
```sh
Step1: Download the Office deployment tool
```
https://www.microsoft.com/en-us/download/details.aspx?id=49117

Step2:
Delete everything from the deployment folder except setup

Step3: Copy the XML sample and key from the link below 

```sh
<Configuration>
  <Add OfficeClientEdition="64"  Channel="PerpetualVL2024">
     <Product ID="ProPlus2024Volume" PIDKEY="	2TDPW-NDQ7G-FMG99-DXQ7M-TX3T2" >
         <Language ID="en-us" />
    </Product>
  </Add>
  <RemoveMSI />
  <Property Name="AUTOACTIVATE" Value="1" />
</Configuration>
```

https://learn.microsoft.com/en-us/office/ltsc/preview/install-ltsc-preview

STep4: Run the command below from cmd prompt as admin from the path where the office deployment tool is saved

```sh
setup /configure configutarion.xml
```
