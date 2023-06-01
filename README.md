# How can I view the Flutter DataTable on Drive using a web browser without downloading?

In this article, we will explore the steps to visualize [Flutter DataGrid](https://www.syncfusion.com/flutter-widgets/flutter-datagrid) hosted on Google Drive without the hassle of downloading and opening files on your local machine.

Initialize the [SfDataGrid](https://pub.dev/documentation/syncfusion_flutter_datagrid/latest/datagrid/SfDataGrid-class.html) widget with all the required properties. The [exportToPdfDocument](https://pub.dev/documentation/syncfusion_flutter_datagrid_export/latest/syncfusion_flutter_datagrid_export/DataGridPdfExportExtensions/exportToPdfDocument.html) method is invoked on the current state of the [key](https://api.flutter.dev/flutter/widgets/Widget/key.html) to export the data from the [SfDataGrid](https://pub.dev/documentation/syncfusion_flutter_datagrid/latest/datagrid/SfDataGrid-class.html) as a [PdfDocument](https://pub.dev/documentation/syncfusion_flutter_pdf/latest/pdf/PdfDocument-class.html). A URL for the PDF is created using [Blob](https://api.flutter.dev/flutter/dart-html/Blob-class.html) and [createObjectUrl](https://api.flutter.dev/flutter/dart-html/Url/createObjectUrl.html), where the PDF bytes are passed along with the MIME type of application/pdf. The [open](https://api.flutter.dev/flutter/dart-html/Window/open.html) method is utilized to open the URL in a new browser tab.

 ```dart
final GlobalKey<SfDataGridState> _key = GlobalKey<SfDataGridState>();

@override
  Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(title: const Text('SfDatagrid Demo')),
        body: Column(children: [
          const Padding(padding: EdgeInsets.only(top: 30)),
          SizedBox(
              child: ElevatedButton.icon(
            onPressed: () async {
              final PdfDocument document =
                  _key.currentState!.exportToPdfDocument();

              final List<int> data = document.saveSync();
              Uint8List bytes = Uint8List.fromList(data);

              final url = html.Url.createObjectUrl(
                  html.Blob([bytes], 'application/pdf'));
              html.window.open(url, '_blank');

              document.dispose();
            },
            icon: const Icon(Icons.print),
            label: const Text('Export DataGrid to PDF'),
          )),
          Expanded(
              child: Card(
            margin: const EdgeInsets.all(30),
            child: SfDataGrid(
                source: _employeeDataSource,
                key: _key,
                columns: getColumns,
                columnWidthMode: ColumnWidthMode.fill),
          ))
        ]));
  }

 ```