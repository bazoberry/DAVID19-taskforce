# DAVID-19Pact LACChain task force

COVID-19PAct® is a task force where different LACChain partners collaborate to lead a campaign to provide useful blockchain-based solutions to combat the spread of the Coronavirus. The goal is to:

* Propose a standard for the forms and the verifiable credentials that are generated by different mobile apps to notarize information about isolation periods.

* Create a single smart contract that will register all the cryptographyc proofs of the credentials so every app whitelisted can register the proofs on it, and everyone can verify them against it.

* Create a map where pseudonymous information about citizens is displayed with different colors depending on if they are health and isolatade/quarantined (green), interruption of isolation/quarantine for a permitted reason (yellow), with symptoms (orange), infected (red), or recovered (purple). The only information that is collected is age, sex, location, and information to notarize.

## 1. Definition of the information to be notarized 

We are proposing to have five different forms to notarize: 

* Confinement [ESP version](https://github.com/lacchain/COVID19-taskforce/blob/master/docs/FORM_CONFINAMIENTO.md) [ENG version](https://github.com/lacchain/COVID19-taskforce/blob/master/docs/CONFINEMENT_FORM.md) 
* Interruption of confinement [ESP version](https://github.com/lacchain/COVID19-taskforce/blob/master/docs/FORM_INTERRUPCION.md) [ENG version](https://github.com/lacchain/COVID19-taskforce/blob/master/docs/CONFINEMENT_INTERRUPTION_FORM.md) 
* Symptoms [ESP version](https://github.com/lacchain/COVID19-taskforce/blob/master/docs/FORM_SINTOMAS.md) [ENG version](https://github.com/lacchain/COVID19-taskforce/blob/master/docs/SYMPTOMS_FORM.md) 
* Infection [ESP version](https://github.com/lacchain/COVID19-taskforce/blob/master/docs/FORM_INFECCION.md) [ENG version](https://github.com/lacchain/COVID19-taskforce/blob/master/docs/INFECTION_FORM.md) 
* Recovery [ESP version](https://github.com/lacchain/COVID19-taskforce/blob/master/docs/FORM_CURA.md) [ENG version](https://github.com/lacchain/COVID19-taskforce/blob/master/docs/RECOVERY_FORM.md)

Click on the links to see them and provide feedback. 

## 2. Data formats and verifiable credential standards 

We are proposing to create a common format aligned with W3C, DIF, EBSI, and others in which the information of the forms provided by the citizens via UI will be embedded and understood by all wallets.  
Additional requirements for the credentials are: 

* They have to be signed by the same private key that corresponds to the app/digital wallet that was used to generate them. 

* The hash of the credential, time-stamp, expiration date, public key of the app that generated it, gender of the person, age of the person, location of the person (adding latitude and longitude), and type of information attested (isolation, interruption of isolation and reason, symptoms, infection, or recovery) has to be send to a common smart contract deployed on the blockchain and described in Section 3. 

* No information other than the indicated in the previous bullet point can be kept by the provider of the app. Therefore, the provider of the app won’t keep a copy of the credentials in its repositories. 

* In order to verify the credentials, the proof mechanism will consist of checking against the smart contract that the hash/id of the credential was registered in that smart contract, the time-stamp, and the expiration date. 

Note: Using the verifiable credential standard implies that credentials shall also have ID, Subject ID, and Status (active, revoked).

## 3. Verification of credentials

We are proposing to deploy a single smart contract in the LACChain Blockchain Network (Hyperledger Besu) where only those apps validated (by the IDB team of LACChain) will be able to write. In this smart contract, therefore, the IDB LACChain team will whitelist the address of those apps validated. This apps will be therefore authorized to register the hashes of the credentials they generate in the smart contract and anyone will be able to verify the credentials against the smart contract. That allows any of the app/digital wallet involved in this collaborative effort to verify credentials issued by others as (i) they all use the same format for the verifiable credentials, and (ii) they all verify them against the same smart contract, and (iii) they trust the IDB to only whitelist trusted apps to register credentials id’s in the smart contract.

In this smart contract, the apps will register the following information of each credential they generate: 

*	Hash of the credential 
*	Time-stamp 
*	Expiration date 
*	Public key of the app that generated it 
*	Sex of the person 
*	Age of the person 
*	Location of the person 
*	Type of information attested (isolation, interruption of isolation and reason, symptoms, infection, or recovery)  

As explained before, the three first items will be used to verify the credential, the fourth item will used to monitor how much is each app being used, and the other items will be used to monitor pseudonymous information as explained in the following Section. 

![Verification of credentials](/docs/verification_covid.png)

## 4. Presentation of the pseudonymized data
 
We are proposing the have an external service listening events to get the public information that has been registered in the blockchain consisting of: 

*	Public key of the app that generated it 
*	Sex of the person 
*	Age of the person 
*	Location of the person 
*	Type of information attested (isolation, interruption of isolation and reason, symptoms, infection, or recovery)  

This service will be subscribed to events in the smart contract. The idea is to create and display a map that plots points of five different colours in the location where the information has been notified. This colours would be: 

*	Green for isolation 
*	Yellow for interruption of isolation 
*	Orange for symptoms 
*	Red for infection 
* Purple for recovery

When one clicks on the points/region, it should be able to see information of the sex/age of the person (it can be on average or individually, depending of the number of people reporting).

We are also proposing to generate dashboards indicating, at least: 

*	How many credentials have been generated. 
*	Where (location of the citizen) are the credentials being generated (and therefore the info notarized). 
*	How many credentials generated each app. 
*	How many credentials have been generated of each type. 
*	A classification by frequency of the reasons why people interrupt isolation (from credential #2). 

![Presentation of credentials](/docs/presentation_covid.png)

## 5. Privacy ensured

Only the citizens own the credentials and have all the information they provide. Nobody else (nor the app provider, the blockchain network, or the map displaying the data) has any information other than: 

*	Sex
*	Age 
*	Location 
*	Type of information attested (isolation, interruption of isolation and reason, symptoms, infection, or recovery) 
 
## 6. Requirements at the application level

Authentication: Apps/digital wallets are required to authenticate citizens using a two-factor authentication. We recommend using something that you have (the phone itself, but receiving an SMS) and something you know (a password). Idemia is also providing free linceses to leverage their biometric proof of live solution via SDK.

DID methods: The implementation chosen is up to the app provider.

Credential proofs: The implementation chosen is up to the app provider.

Frequency of generation of credentials: Only one credential of each type can be generated per person per day. Except the credential "isolation" that can be generated twice, before and after the generation of the credential "interruption of isolation".

Compatibility of credentials: The credentials "Isolation" and "Interruption of isolation" can't be active at the same time for the same person. One one is generated, the other has to be changed its status to "revoked". The credentials "symptoms", "infection", and "ecovery" can't be active at the same time for the same person. One one is generated, the other ones have to be changed their status to "revoked".

## 7. Avoidance of duplication of same credentials for same users in different applications (optional)

Applications will generate an ID by hashing personal data of the people. These information will not be registered in a smart contract nor linked to any other information provided. Only the hash will be shared privately between applications collaborating in this effort in order to notify citizens not to re-register the same info if it was already registered in the other app.





