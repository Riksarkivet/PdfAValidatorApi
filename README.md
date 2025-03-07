# Codeuctivity.PdfAValidator

[![Build](https://github.com/Codeuctivity/PdfAValidatorApi/actions/workflows/dotnet.yml/badge.svg)](https://github.com/Codeuctivity/PdfAValidatorApi/actions/workflows/dotnet.yml) [![Nuget](https://img.shields.io/nuget/v/Codeuctivity.PdfaValidator.svg)](https://www.nuget.org/packages/Codeuctivity.PdfAValidator/) [![Donate](https://img.shields.io/static/v1?label=Paypal&message=Donate&color=informational)](https://www.paypal.com/donate?hosted_button_id=7M7UFMMRTS7UE)

PdfAValidatorApi wraps calls to [VeraPdf](http://www.preforma-project.eu/pdfa-conformance-checker.html) in a .net standard assembly and as WebApi. Access VeraPdf from your unit tests or integrate it into your micro architecture. Works on Windows, Linux and MacOs.

Sample - e.g. use it in your unit test to check compliance of some pdf:

```csharp
public static async Task ShouldDetectCompliantPdfA()
{
    using var pdfAValidator = new PdfAValidator();
    var result = await pdfAValidator.ValidateAsync("./TestPdfFiles/FromLibreOffice.pdf");
    Assert.True(result);
}
```

Sample - e.g. use it in your unit test to check the used sub standard of some pdf:

```csharp
public static async Task ShouldGetDetailedReportFromPdfA()
{
    using var pdfAValidator = new PdfAValidator();
    var result = await pdfAValidator.ValidateWithDetailedReportAsync("./TestPdfFiles/FromLibreOffice.pdf");
    Assert.True(result.Jobs.Job.ValidationReport.IsCompliant);
    Assert.True(result.Jobs.Job.ValidationReport.ProfileName == "PDF/A-1A validation profile");
}
```

Sample - e.g. use it in your unit test to check [PDF meta data](https://docs.verapdf.org/cli/feature-extraction/):

```csharp
 public static async Task ShouldGetFeaturesReportWhenAskingForIt()
 {
     using var pdfAValidator = new PdfAValidator();
     var result = await pdfAValidator.ValidateWithDetailedReportAsync("./TestPdfFiles/FromLibreOffice.pdf", "--extract");
     var producerEntry = result.Jobs.Job.FeaturesReport.InformationDict.Entries.Single(e => e.Key == "Producer");
     Assert.Equal("LibreOffice 6.1", producerEntry.Value);
 }
```

## Demo OpenApi - PdfAValidatorWebApi

Give <https://pdfavalidator.azurewebsites.net> a try, but don't be disappointed if it is off-line. The demo azure account is running on limited budget.

## Dependencies

### Windows

Everything comes with the nuget package

### Ubuntu

Current PdfAValidatorApi depends on openjdk-11-jre and some .net version.

```bash
sudo apt install openjdk-11-jre
sudo update-alternatives --config java
```

## Development dependencies

### Setup .net sdk ubuntu 22.04

```bash
sudo apt install dotnet6
```
