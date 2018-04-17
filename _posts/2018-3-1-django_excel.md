# Django excel

## Installation
* ```pip install django-excel```

## Django setting
```python
FILE_UPLOAD_HANDLERS = ("django_excel.ExcelMemoryFileUploadHandler",
                        "django_excel.TemporaryExcelFileUploadHandler")
```
