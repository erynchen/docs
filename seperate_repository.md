# Seperate maven repository for different project

maven defalut repository located at
`C:\Users\...\.m2\repository`

However, sometimes we need different maven version and seperate repository as well.  

So, we need:

##### step 1: create new repo at your local, like 
`C:\Users\...\.m2\repository-lab`

##### step 2: create another setting.xml, which include:
> <localRepository>C:\Users\...\.m2\repository-lab</localRepository>

##### step 3: set up your setting.xml to your eclipse or STS. 
normally, you could config setting.xml at: 
> Preference -> Maven -> User Settings 
