# VBA-challenge
VBA Homework
Sub VBA_Challenge()

    ' Set an initial variable for holding the ticker name
    Dim Ticker_Name As String
    Ticker_Name = " "
    
    ' Set an initial integer for the ticker counter
    Dim Ticker_Counter As Integer
    Ticker_Counter = 0
    
    ' Set an initial variable for the yearly change
    Dim Yearly_Change As Double
    Yearly_Change = 0
    
    ' Set an initial variable for the percent change
    Dim Percent_Change As Double
    Percent_Change = 0
    
    ' Set an initial variable for holding the total per ticker name
    Dim Total_Stock_Volume As Double
    Total_Stock_Volume = 0
    
    ' Set an initial variable for the open price
    Dim Open_Price As Double
    Open_Price = 0
    
    ' Set an initial variable for the close price
    Dim Close_Price As Double
    Close_Price = 0
    
    ' Set an initial variable for the greatest % increase
    Dim Greatest_Percent_Increase As Double
    Greatest_Percent_Increase = 0
    
    ' Set an initial variable for the greatest % decrease
    Dim Greatest_Percent_Decrease As Double
    Greatest_Percent_Decrease = 0
    
    ' Set an initial variable for the greatest total vol
    Dim Greatest_Total_Volume As Double
    Greatest_Total_Volume = 0
    
    
    'loop through each worksheet in the workbook
    For Each ws In Worksheets
    
        ' Make the worksheet active
        ws.Activate
        
    ' Set last row as long
    Dim LastRow As Long
    
    ' Keep track of the location for each ticker in the summary table
    Dim Summary_Table_Row As Long
    Summary_Table_Row = 2

    ' Label the summary table columns
    ws.Cells(1, 9).Value = "Ticker Name"
    ws.Cells(1, 10).Value = "Yearly Change"
    ws.Cells(1, 11).Value = "Percent Change"
    ws.Cells(1, 12).Value = "Total Stock Volume"

    ' Find last row
    LastRow = Cells(Rows.Count, 1).End(xlUp).Row
    
        ' Loop through all ticker code stock volumes
        For i = 2 To LastRow
        
            ' Set the ticker name
            Ticker_Name = Cells(i, 1).Value
                        
            ' Set open price to 0 and set location
                If Open_Price = 0 Then
                    Open_Price = Cells(i, 3).Value
                End If
            
            ' Check if we are still within the same ticker name, if it is not...
            If Cells(i + 1, 1).Value <> Cells(i, 1) Then

            ' Add to the ticker counter and set to column I
            Ticker_Counter = Ticker_Counter + 1
            Cells(Ticker_Counter + 1, 9) = Ticker_Name
            
            ' Add to the total stock volume
            Total_Stock_Volume = Total_Stock_Volume + Cells(i, 7).Value
            
            ' Set close price to 0
            Close_Price = Cells(i, 6)
            
            ' Calculate yearly change
            Yearly_Change = Close_Price - Open_Price
            
            ' Set the ticker counter to column J
            ws.Cells(Ticker_Counter + 1, 10).Value = Yearly_Change
            
            ' Color cells if > 0 = GREEN - good or color cells if < 0 = RED - bad
                If (Yearly_Change > 0) Then
                    ws.Cells(Ticker_Counter + 1, 10).Interior.ColorIndex = 4
                ElseIf (Yearly_Change < 0) Then
                    ws.Cells(Ticker_Counter + 1, 10).Interior.ColorIndex = 3
                End If
                
                If Open_Price = 0 Then
                    Percent_Change = 0
                Else
                    Percent_Change = (Yearly_Change / Open_Price)
                End If
            
            ' Print the ticker name in the Summary Table
            Range("I" & Summary_Table_Row).Value = Ticker_Name
            
            ' Print the yearly change in the Summary Table
            ws.Range("J" & Summary_Table_Row).Value = Yearly_Change
            
            ' Print the percent change in the Summary Table
            Range("K" & Summary_Table_Row).Value = Percent_Change
            
            ' Set the style to percent
            Range("K" & Summary_Table_Row).Style = "Percent"
            
            ' Print the total stock volume to the Summary Table
            Range("L" & Summary_Table_Row).Value = Total_Stock_Volume

            ' Add one to the summary table row
            Summary_Table_Row = Summary_Table_Row + 1
            
            ' Reset the open price
            Open_Price = 0
            
            ' Reset the total stock volume
            Total_Stock_Volume = 0

            ' If the cell immediately following a row is the same name...
        Else

      ' Add to the total stock volume
      Total_Stock_Volume = Total_Stock_Volume + Cells(i, 7).Value

    End If

  Next i
    Cells(2, 15).Value = "Greatest % Increase"
    Cells(3, 15).Value = "Greatest % Decrease"
    Cells(4, 15).Value = "Greatest Total Volume"
    Cells(1, 16).Value = "Ticker"
    Cells(1, 17).Value = "Value"
    
    ' Find the last row for column I
    LastRow = Cells(Rows.Count, "I").End(xlUp).Row
    
    Greatest_Percent_Increase = Cells(2, 11).Value
    Greatest_Percent_Increase_Ticker = Cells(2, 9).Value
    Greatest_Percent_Decrease = Cells(2, 11).Value
    Greatest_Percent_Decrease_Ticker = Cells(2, 9).Value
    Greatest_Stock_Volume = Cells(2, 12).Value
    Greatest_Stock_Volume_Ticker = Cells(2, 9).Value
    
        For i = 2 To LastRow
            
            If (Cells(i, 11).Value > Greatest_Percent_Increase) Then
                Greatest_Percent_Increase = Cells(i, 11).Value
                Greatest_Percent_Increase_Ticker = Cells(i, 9).Value
            End If
            
            If (Cells(i, 11).Value < Greatest_Percent_Decrease) Then
                Greatest_Percent_Decrease = Cells(i, 11).Value
                Greatest_Percent_Decrease_Ticker = Cells(i, 9).Value
            End If
            
            If (Cells(i, 12).Value > Greatest_Stock_Volume) Then
                Greatest_Stock_Volume = Cells(i, 12).Value
                Greatest_Stock_Volume_Ticker = Cells(i, 9).Value
            End If
            
        Next i
        
        Range("P2").Value = Format(Greatest_Percent_Increase_Ticker, "Percent")
        Range("Q2").Value = Format(Greatest_Percent_Increase, "Percent")
        Range("P3").Value = Format(Greatest_Percent_Decrease_Ticker, "Percent")
        Range("Q3").Value = Format(Greatest_Percent_Decrease, "Percent")
        Range("P4").Value = Greatest_Stock_Volume_Ticker
        Range("Q4").Value = Greatest_Stock_Volume
        
    Next ws
    
End Sub
