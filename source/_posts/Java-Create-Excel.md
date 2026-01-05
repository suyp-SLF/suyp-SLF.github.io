---
title: Java Create Excel
date: 2023-02-22 09:18:16
tags:
  - Java
categories:
  - Java
  - Excel
cover: /images/cover.jpg  # 站内图片
---

# Code Excel
```Java
//返回excel的方法
private Workbook setCellFont(Row[] rows, PageMeta template, Map<String, Set<String>> colorChange){
    Integer MAXLINE = Integer.MAX_VALUE;
    GridColumnMeta[] titles = ((GridMeta) template.getAreaMeta("wa_data")).getItems();
    Workbook wb = new XSSFWorkbook();
    Sheet sheet = null;
    for (int i = 0; i < rows.length; i++) {
        org.apache.poi.ss.usermodel.Row EXCEL_row =null;
        if(i % MAXLINE == 0){
            sheet = wb.createSheet(String.valueOf("sheet" + i / MAXLINE));
            EXCEL_row = sheet.createRow(i%MAXLINE);
            for (int j = 0; j < titles.length; j++) {
                Boolean visible = titles[j].isVisible();
                if (visible) {
                    String EXCEL_title_name = titles[j].getLabel();
                    EXCEL_row.createCell(j).setCellValue(EXCEL_title_name);
                }
            }
        }
        EXCEL_row = sheet.createRow((i%MAXLINE) + 1);
        Map<String, Cell> cells = rows[i].getValues();
        String pk_wa_data = cells.get("pk_wa_data").getValue().toString();

        //template.get

        for (int j = 0; j < titles.length; j++) {
            Boolean visible = titles[j].isVisible();
            if (visible){
                String EXCEL_title_code = titles[j].getAttrcode();
                if(!"ts".equalsIgnoreCase(EXCEL_title_code) && !"pk_wa_data".equalsIgnoreCase(EXCEL_title_code)) {

                    String EXCEL_value = "";

                    Cell cell = cells.get(EXCEL_title_code);
                    if (cell != null && cell.getValue() != null && cell.getScale() > 0) {
                        EXCEL_value = new BigDecimal(cell.getValue().toString()).setScale(cell.getScale()).toString();
                    } else if (cell != null && cell.getValue() != null) {
                        EXCEL_value = cell.getValue().toString();
                    }
                    if (colorChange.get(EXCEL_title_code) != null && colorChange.get(EXCEL_title_code).contains("pk_wa_data=" + pk_wa_data)) {
                        org.apache.poi.ss.usermodel.Cell EXCEL_cell = EXCEL_row.createCell(j);
                        CellStyle cellStyle = wb.createCellStyle();
                        Font redFont = wb.createFont();

                        redFont.setColor(Font.COLOR_RED);
                            //
                        EXCEL_cell.setCellValue(EXCEL_value);
                            //
                        cellStyle.setFont(redFont);
                        EXCEL_cell.setCellStyle(cellStyle);
                    } else {
                        EXCEL_row.createCell(j).setCellValue(EXCEL_value);
                    }

                }else {
                    continue;
                }
            }else {
                continue;
            }
            //org.apache.poi.ss.usermodel.Cell EXECEL_cell = EXCEL_row.createCell(j);
            //EXECEL_cell.setCellValue(cells.get);
        }
    }
    return wb;
}
```