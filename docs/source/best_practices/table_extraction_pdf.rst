Table Extraction from PDF
=========================

This section describes two methods for extracting tables from PDF files.

.. note::

    To extract tables from any documents, set the ``strategy`` parameter to ``hi_res`` for both methods below.

Method 1: Using `partition_pdf`
-------------------------------

To extract the tables from PDF files using the `partition_pdf <https://unstructured-io.github.io/unstructured/core/partition.html#partition-pdf>`__, set the ``infer_table_structure`` parameter to ``True`` and ``strategy`` parameter to ``hi_res``.

**Usage**

.. code-block:: python

    from unstructured.partition.pdf import partition_pdf

    fname = "example-docs/layout-parser-paper.pdf"

    elements = partition_pdf(filename=fname,
                             infer_table_structure=True,
                             strategy='hi_res',
               )

    tables = [el for el in elements if el.category == "Table"]

    print(tables[0].text)
    print(tables[0].metadata.text_as_html)

Method 2: Using Auto Partition or Unstructured API
--------------------------------------------------

For extracting tables from PDFs using `auto partition <https://unstructured-io.github.io/unstructured/core/partition.html#partition>`__ or `Unstructured API parameters <https://unstructured-io.github.io/unstructured/apis/api_parameters.html>`__ , set the ``pdf_infer_table_structure`` parameter to **True** and ``strategy`` parameter to ``hi_res``.

.. warning::

    You may get a warning when the ``pdf_infer_table_structure`` parameter is set to **True** AND **pdf** is included in the list of ``skip_infer_table_types`` parameter. However, this function will still extract the tables from PDF despite the conflict.

**Usage: Auto Partition**

.. code-block:: python

    from unstructured.partition.auto import partition

    filename = "example-docs/layout-parser-paper.pdf"

    elements = partition(filename=filename,
                         pdf_infer_table_structure=True,
                         strategy='hi_res',
               )

    tables = [el for el in elements if el.category == "Table"]

    print(tables[0].text)
    print(tables[0].metadata.text_as_html)


**Usage: API Parameters**

.. code-block:: bash

     curl -X 'POST' \
      'https://api.unstructured.io/general/v0/general' \
      -H 'accept: application/json' \
      -H 'Content-Type: multipart/form-data' \
      -F 'files=@sample-docs/layout-parser-paper.pdf' \
      -F 'strategy=hi_res' \
      -F 'pdf_infer_table_structure=true' \
      | jq -C . | less -R