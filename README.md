# SAVE_US


CREATE TABLE adr_report
(
	report_date          DATE NULL,
	report_num           INT(8) NOT NULL,
	sym_code             INT(8) NULL,
	sym_name             VARCHAR(255) NULL,
	p_num                INT(8) NULL,
	p_name               VARCHAR(20) NULL,
	take_dosage          FLOAT NULL,
	take_freq            VARCHAR(255) NULL,
	take_start_date      DATE NULL,
	take_end_date        DATE NULL,
	take_days            INT(3) NULL,
	take_stop            TINYINT(4) NULL,
	take_recover         TINYINT(4) NULL,
	take_retake          TINYINT(4) NULL,
	take_react           TINYINT(4) NULL,
	take_supplier        VARCHAR(255) NULL,
	edi_code             INT(9) NULL,
	d_name               VARCHAR(255) NULL
);

ALTER TABLE adr_report
ADD PRIMARY KEY (report_num);

CREATE TABLE disease
(
	disease_code         VARCHAR(255) NOT NULL,
	disease_name_kr      VARCHAR(255) NULL,
	disease_name_en      VARCHAR(50) NULL
);

ALTER TABLE disease
ADD PRIMARY KEY (disease_code);

CREATE TABLE drug
(
	edi_code             INT(9) NOT NULL,
	d_name               VARCHAR(255) NOT NULL,
	d_ingredient         TEXT NULL,
	d_payment            VARCHAR(100) NULL,
	atc_code             VARCHAR(50) NULL,
	d_type               VARCHAR(255) NULL,
	d_otc                VARCHAR(50) NULL
);

ALTER TABLE drug
ADD PRIMARY KEY (edi_code,d_name);

CREATE TABLE drug_dur
(
	dur_combine          TEXT NULL,
	dur_age              TEXT NULL,
	dur_pregnant         TEXT NULL,
	dur_old              TEXT NULL,
	dur_dosage           TEXT NULL,
	dur_period           TEXT NULL,
	dur_split            TEXT NULL,
	dur_blood_donate     TEXT NULL,
	edi_code             INT(9) NOT NULL,
	d_name               VARCHAR(255) NOT NULL
);

ALTER TABLE drug_dur
ADD PRIMARY KEY (edi_code,d_name);

CREATE TABLE drug_effect
(
	edi_code             INT(9) NOT NULL,
	d_name               VARCHAR(255) NOT NULL,
	effect               TEXT NULL
);

ALTER TABLE drug_effect
ADD PRIMARY KEY (edi_code,d_name);

CREATE TABLE meta_rwd
(
	dra_pt               VARCHAR(255) NOT NULL,
	meta_rr              FLOAT NULL,
	meta_or              FLOAT NULL,
	meta_p               FLOAT NULL,
	edi_code             INT(9) NOT NULL,
	d_name               VARCHAR(255) NOT NULL,
	atc_code             CHAR(18) NULL,
	�ּ��и�             CHAR(18) NULL
);

ALTER TABLE meta_rwd
ADD PRIMARY KEY (dra_pt,edi_code,d_name);

CREATE TABLE meta_rwd_drug
(
	dra_pt               VARCHAR(255) NOT NULL,
	edi_code             INT(9) NOT NULL,
	d_name               VARCHAR(255) NOT NULL
);

ALTER TABLE meta_rwd_drug
ADD PRIMARY KEY (dra_pt,edi_code,d_name);

CREATE TABLE patient
(
	p_num                INT(8) NOT NULL,
	p_name               VARCHAR(20) NOT NULL,
	p_birth              DATE NULL,
	p_sex                TINYINT(4) NULL,
	p_age                FLOAT NULL,
	p_height             FLOAT NULL,
	p_weight             FLOAT NULL,
	p_pregnant           TINYINT(4) NULL,
	p_medhistory         VARCHAR(50) NULL
);

ALTER TABLE patient
ADD PRIMARY KEY (p_num,p_name);

CREATE TABLE prescription
(
	pre_num              INT(8) NOT NULL,
	pre_dosage           FLOAT NULL,
	pre_freq             VARCHAR(255) NULL,
	pre_days             INT(3) NULL,
	pre_date             DATE NOT NULL,
	pre_method           VARCHAR(255) NULL,
	edi_code             INT(9) NOT NULL,
	d_name               VARCHAR(255) NOT NULL,
	p_num                INT(8) NULL,
	p_name               VARCHAR(255) NULL,
	disease_code         VARCHAR(255) NULL
);

ALTER TABLE prescription
ADD PRIMARY KEY (pre_num,pre_date);

CREATE TABLE symptom
(
	sym_code             INT(8) NOT NULL,
	sym_name             VARCHAR(255) NOT NULL,
	sym_onset_time       TIME NULL,
	sym_date             DATE NULL,
	sym_recover          TINYINT(4) NULL
);

ALTER TABLE symptom
ADD PRIMARY KEY (sym_code,sym_name);

ALTER TABLE adr_report
ADD CONSTRAINT R_12 FOREIGN KEY (sym_code, sym_name) REFERENCES symptom (sym_code, sym_name);

ALTER TABLE adr_report
ADD CONSTRAINT R_13 FOREIGN KEY (p_num, p_name) REFERENCES patient (p_num, p_name);

ALTER TABLE adr_report
ADD CONSTRAINT R_30 FOREIGN KEY (edi_code, d_name) REFERENCES drug (edi_code, d_name);

ALTER TABLE drug_dur
ADD CONSTRAINT R_15 FOREIGN KEY (edi_code, d_name) REFERENCES drug (edi_code, d_name);

ALTER TABLE drug_effect
ADD CONSTRAINT R_19 FOREIGN KEY (edi_code, d_name) REFERENCES drug (edi_code, d_name);

ALTER TABLE meta_rwd
ADD CONSTRAINT R_18 FOREIGN KEY (edi_code, d_name) REFERENCES drug (edi_code, d_name);

ALTER TABLE meta_rwd_drug
ADD CONSTRAINT R_27 FOREIGN KEY (dra_pt, edi_code, d_name) REFERENCES meta_rwd (dra_pt, edi_code, d_name);

ALTER TABLE meta_rwd_drug
ADD CONSTRAINT R_28 FOREIGN KEY (edi_code, d_name) REFERENCES drug (edi_code, d_name);

ALTER TABLE prescription
ADD CONSTRAINT R_14 FOREIGN KEY (p_num, p_name) REFERENCES patient (p_num, p_name);

ALTER TABLE prescription
ADD CONSTRAINT R_17 FOREIGN KEY (disease_code) REFERENCES disease (disease_code);

ALTER TABLE prescription
ADD CONSTRAINT R_7 FOREIGN KEY (edi_code, d_name) REFERENCES drug (edi_code, d_name);
