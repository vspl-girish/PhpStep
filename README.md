# PhpStep

PHP Spreadsheet Template Engine 

Thanks to PHPOffice/PhpSpreadsheet, we are able to generate various types of spreadsheets directly from php.
PhpStep is a humble attempt to automate data populated spreadsheets.

In this project, we put together an existing xslx file containing some template attributes and a structured model 
or json data to output a ready to use spreadsheet with user readable data.

### Dependencies

* PHPOffice/PhpSpreadsheet
* PHP 7.2

### Installation and Usage

Publish to composer pending

Currently, use the git repository for usage.

## Supported tags for template

*   $F{field_name} - A field/property in the class/data source
*   $Each{Iterator} - Any collection object/array implementing Iterator interface 

### Sample Code

First, create an sample.xlsx file with following structure:

|      A        |       B       |
| ------------- | ------------- |
| $F{message}   |               |
|               |               |
| Country       | Population    |
| $Each{stats}  |               |
| ${country}    | ${population} |


You can also apply various formats to the cells and also create normal formulas.

```php

require '../vendor/autoload.php';

$model = new \stdClass();
$model->message = 'Hello World!';
$model->stats = [
    ['country' => 'India', 'population' => 1300]
    ['country' => 'USA', 'population' => 330,
    ['country' => 'Russia', 'population' => 145
];

$reader = PhpOffice\PhpSpreadsheet\IOFactory::createReader('Xlsx');
$ss = $reader->load("sample.xlsx");
$worksheet = $ss->getActiveSheet();

$re = new PhpStep\RenderWorksheet();
$re->applyData($worksheet, $model);

$writer = PhpOffice\PhpSpreadsheet\IOFactory::createWriter($ss, 'Xlsx');
$writer->save('sampleResult.xlsx');

```
For complex methods, refer to test/TestRenderWorksheet.php and testData.xlsx