# Static SSR Client Validation Opt-Out

This repository contains a sample application and validation guidance for ASP.NET Core Issue #69528, demonstrating per-form and app-wide opt-out of client-side validation in static server-side rendering (static SSR) Blazor forms.

## Repository Purpose

The sample validates client-side and server-side validation behavior for static SSR forms that use `DataAnnotationsValidator`. The validation covers:

- Client-side validation enabled by default for a static SSR form
- Per-form opt-out using `DataAnnotationsValidator.DisableClientValidation`
- App-wide opt-out using `RazorComponentsServiceOptions.DisableClientValidation`
- Server validation after an opted-out form posts invalid data
- Successful server submissions from default and opted-out forms
- Independent form mapping with distinct form names
- Inputs without native HTML `required` constraints, ensuring that browser behavior doesn't mask the Blazor validation path

## Contents

- Static SSR Blazor Web App
- Two independent validation forms
- Startup-selectable app-wide client validation opt-out
- On-page validation instructions and visible server submission results

## Prerequisites

- .NET SDK 11.0.100-rc.1.26425.128 or later .NET 11 RC1 SDK
- Visual Studio 2026 Preview (or later)

The sample uses a `global.json` file that pins the SDK version to .NET 11 RC1 to ensure consistent builds and reproducible validation results.

## Repository Structure

```plaintext
blazor-static-ssr-optout-validation
│
├── Samples
│   └── StaticSSROptOutValidation
├── Evidence
    └── ValidationReport
    └── Screenshots
```

## Building and Running the Sample

```sh
git clone https://github.com/BrundhaVelusamy/blazor-static-ssr-optout-validation.git

cd blazor-static-ssr-optout-validation/Samples/StaticSSROptOutValidation

dotnet restore
dotnet build
dotnet run
```

## Application URLs

The HTTP launch profile uses:

```plaintext
http://localhost:5288
```

The HTTPS launch profile uses:

```plaintext
https://localhost:7249
http://localhost:5288
```

Use the URL displayed by `dotnet run` if it differs.

## Validation Steps

### App-wide opt-out disabled

1. Start the sample with the default configuration.
2. Open the browser developer tools and select the **Network** tab.
3. Submit the **Default client validation** form with an empty name. Confirm that the field message appears without a POST.
4. Submit the **Per-form client validation opt-out** form with an empty name. Confirm that a POST occurs and the server validation message is returned.
5. Enter a valid name of 3 to 20 characters in each form and confirm that both submissions reach the server and display success results.

### App-wide opt-out enabled

1. Restart the sample with `--DisableClientValidation true`.
2. Submit both forms with empty names. Confirm that both submissions issue POST requests and return server validation messages.
3. Enter valid names and confirm that both forms display successful server submission results.

## Expected Behavior

- With the app-wide opt-out disabled, an invalid default form is blocked in the browser without a POST.
- The per-form opted-out form posts invalid data and displays the server validation result.
- With the app-wide opt-out enabled, invalid submissions from both forms reach the server.
- Valid submissions complete successfully from both forms with either global setting.
