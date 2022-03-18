# Partial-Grouping-issues-M-Language

# Enables dynamic grouping and summing of Items

<img width="398" alt="image" src="https://user-images.githubusercontent.com/84840321/158978395-9a3dfed7-77ca-4150-91c2-6d5a945f51c1.png">

# Implemented with Power Query

let

    Source = Excel.CurrentWorkbook(){[Name="Table1"]}[Content],
    
    #"Step1:Removed Columns" = Table.RemoveColumns(Source,{"SubItem"}),
    
    #"Step2:Sum by groups" = Table.Group(#"Step1:Removed Columns","Item",{"Total",each List.Sum([Number])}),
    
    #"Step3:Selected (ABC)" = Table.SelectRows(#"Step2:Sum by groups", each ([Item] = "A" or [Item] = "B" or [Item] = "C")),
    
    #"Step4:Create a Total Table" = Table.FromRecords({[Item="Total",Total= List.Sum(#"Step3:Selected (ABC)"[Total])]}),
    
    #"Step5:TableCombine" = #"Step3:Selected (ABC)"&#"Step4:Create a Total Table",
    
    #"Step6:SelectedOtherItems" = Table.SelectRows(#"Step2:Sum by groups", each ([Item] <> "A" and [Item] <> "B" and [Item] <> "C")),
    
    #"Step7:Create a Total for otherItem" = Table.FromRecords({[Item="Other",Total= List.Sum(#"Step6:SelectedOtherItems"[Total])]}),
    
    #"Step8:CombineAll Tables" = #"Step5:TableCombine"& #"Step7:Create a Total for otherItem"
    
in

    #"Step8:CombineAll Tables"
    
