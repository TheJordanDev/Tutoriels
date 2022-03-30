> D'abord, il faut faire les tables.

```sql
SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0;
SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0;
SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION';

CREATE SCHEMA IF NOT EXISTS `photobank` DEFAULT CHARACTER SET utf8 ;
USE `photobank` ;

DROP TABLE IF EXISTS `photobank`.`TypeAppareil` ;
CREATE TABLE IF NOT EXISTS `photobank`.`TypeAppareil` (
  `typeAppareil` VARCHAR(100) NOT NULL,
  PRIMARY KEY (`typeAppareil`))
ENGINE = InnoDB;

DROP TABLE IF EXISTS `photobank`.`Photographe` ;
CREATE TABLE IF NOT EXISTS `photobank`.`Photographe` (
  `mailPhotographe` VARCHAR(320) NOT NULL,
  `nom` VARCHAR(20) NULL,
  `prenom` VARCHAR(20) NULL,
  `password` VARCHAR(255) NULL,
  PRIMARY KEY (`mailPhotographe`))
ENGINE = InnoDB;

DROP TABLE IF EXISTS `photobank`.`Photo` ;
CREATE TABLE IF NOT EXISTS `photobank`.`Photo` (
  `idPhoto` INT NOT NULL AUTO_INCREMENT,
  `lien` VARCHAR(255) NULL,
  `date` DATETIME NULL,
  `refPhotographe` VARCHAR(45) NULL,
  `refAppareil` VARCHAR(100) NULL,
  PRIMARY KEY (`idPhoto`),
  INDEX `photo_deviceType_idx` (`refAppareil` ASC) VISIBLE,
  INDEX `photo_photographe_idx` (`refPhotographe` ASC) VISIBLE,
  CONSTRAINT `photo_deviceType`
    FOREIGN KEY (`refAppareil`)
    REFERENCES `photobank`.`TypeAppareil` (`typeAppareil`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `photo_photographe`
    FOREIGN KEY (`refPhotographe`)
    REFERENCES `photobank`.`Photographe` (`mailPhotographe`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;

DROP TABLE IF EXISTS `photobank`.`Utilisateur` ;
CREATE TABLE IF NOT EXISTS `photobank`.`Utilisateur` (
  `mailUtilisateur` VARCHAR(320) NOT NULL,
  `motdepasse` VARCHAR(255) NULL,
  PRIMARY KEY (`mailUtilisateur`))
ENGINE = InnoDB;

DROP TABLE IF EXISTS `photobank`.`Notation` ;
CREATE TABLE IF NOT EXISTS `photobank`.`Notation` (
  `refUtilisateur` VARCHAR(320) NOT NULL,
  `refPhoto` INT NOT NULL,
  `note` INT NULL,
  PRIMARY KEY (`refUtilisateur`, `refPhoto`),
  INDEX `notation_photo_idx` (`refPhoto` ASC) VISIBLE,
  CONSTRAINT `notation_user`
    FOREIGN KEY (`refUtilisateur`)
    REFERENCES `photobank`.`Utilisateur` (`mailUtilisateur`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `notation_photo`
    FOREIGN KEY (`refPhoto`)
    REFERENCES `photobank`.`Photo` (`idPhoto`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;

DROP TABLE IF EXISTS `photobank`.`Tag` ;
CREATE TABLE IF NOT EXISTS `photobank`.`Tag` (
  `libelleTag` VARCHAR(45) NOT NULL,
  PRIMARY KEY (`libelleTag`))
ENGINE = InnoDB;


DROP TABLE IF EXISTS `photobank`.`APourTag` ;
CREATE TABLE IF NOT EXISTS `photobank`.`APourTag` (
  `refPhoto` INT NOT NULL,
  `refTag` VARCHAR(45) NOT NULL,
  PRIMARY KEY (`refPhoto`, `refTag`),
  INDEX `atag_tag_idx` (`refTag` ASC) VISIBLE,
  CONSTRAINT `atag_photo`
    FOREIGN KEY (`refPhoto`)
    REFERENCES `photobank`.`Photo` (`idPhoto`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `atag_tag`
    FOREIGN KEY (`refTag`)
    REFERENCES `photobank`.`Tag` (`libelleTag`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;

DROP TABLE IF EXISTS `photobank`.`Tarif` ;
CREATE TABLE IF NOT EXISTS `photobank`.`Tarif` (
  `noteMoyenne` DECIMAL(2) NOT NULL,
  `prix` DECIMAL(2) NULL,
  PRIMARY KEY (`noteMoyenne`))
ENGINE = InnoDB;


SET SQL_MODE=@OLD_SQL_MODE;
SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS;
SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS;
```

> Il faut aussi insérer des valeurs.

```sql

SET NAMES utf8;
SET time_zone = '+00:00';
SET foreign_key_checks = 0;
SET sql_mode = 'NO_AUTO_VALUE_ON_ZERO';

INSERT INTO `Utilisateur` (`mailUtilisateur`, `motdepasse`) VALUES
('mail1@gmail.com',	'root'),
('mail2@gmail.com',	'root'),
('mail3@gmail.com',	'root'),
('mail4@gmail.com',	'root');

INSERT INTO `Tag` (`libelleTag`) VALUES
('Nature');

INSERT INTO `TypeAppareil` (`typeAppareil`) VALUES
('CANON EOS M50'),
('Sony Alpha 7 III');

INSERT INTO `Photographe` (`mailPhotographe`, `nom`, `prenom`, `password`) VALUES
('photographe@gmail.com',	'Photo',	'Graphe',	'root');

INSERT INTO `Photo` (`idPhoto`, `lien`, `date`, `refPhotographe`, `refAppareil`) VALUES
(1,	'nature1.png',	'2022-03-30 10:05:57',	'photographe@gmail.com',	'Sony Alpha 7 III'),
(2,	'nature2.png',	'2022-03-30 10:15:52',	'photographe@gmail.com',	'Sony Alpha 7 III'),
(3,	'nature3.png',	'2022-03-30 11:04:10',	'photographe@gmail.com',	'Sony Alpha 7 III');

INSERT INTO `APourTag` (`refPhoto`, `refTag`) VALUES
(1,	'Nature');

INSERT INTO `Notation` (`refUtilisateur`, `refPhoto`, `note`) VALUES
('mail1@gmail.com',	1,	3),
('mail1@gmail.com',	2,	3),
('mail2@gmail.com',	1,	10),
('mail2@gmail.com',	2,	10),
('mail2@gmail.com',	3,	3),
('mail3@gmail.com',	1,	6),
('mail3@gmail.com',	2,	6),
('mail4@gmail.com',	1,	10),
('mail4@gmail.com',	2,	10);
```

> Pour récupérer les photos avec la meilleure note, il faut utiliser d'abord initialiser une vue. Cela nous permettra de stocker des valeurs utiles pour la requête finale.

```sql
CREATE VIEW PhotosMoyennes as (SELECT refPhoto, AVG(note) AS noteMoyenne FROM Notation GROUP BY refPhoto);
```

> Puis récupérer les photos les mieux notées avec :

```sql
SELECT refPhoto, noteMoyenne from PhotosMoyennes WHERE noteMoyenne= (SELECT MAX(noteMoyenne) from PhotosMoyennes);
```


