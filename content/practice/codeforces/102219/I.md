---
title: "CF 102219I - Sụp đổ hay không sụp đổ"
description: "Con đường được thể hiện bằng lưới 3 x 10 ký tự. RoboTaxi chiếm ô được đánh dấu = và chỉ có thể tiếp tục đi thẳng dọc theo hàng hiện tại. Các ô còn lại hoặc để trống, biểu thị bằng ., hoặc chướng ngại vật được biểu thị bằng H, T, P."
date: "2026-08-17T22:59:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102219
codeforces_index: "I"
codeforces_contest_name: "2019 ICPC Malaysia National"
rating: 0
weight: 102219
solve_time_s: 312
verified: false
draft: false
---

[CF 102219I - Sụp đổ hay không sụp đổ](https://codeforces.com/problemset/problem/102219/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 12s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Con đường được thể hiện bằng lưới 3 x 10 ký tự. RoboTaxi chiếm ô được đánh dấu`=`và nó chỉ có thể tiếp tục đi thẳng dọc theo hàng hiện tại của nó. Các ô còn lại trống, được biểu thị bằng`.`, hoặc chướng ngại vật đại diện bởi`H`,`T`, Và`P`. 

Vì xe taxi không bao giờ chuyển làn hoặc đảo hướng nên chỉ có các ô ở ngay bên phải của`=`trong cùng một hàng có thể ảnh hưởng đến kết quả. Trong số các ô đó, chướng ngại vật đầu tiên gặp phải là ô bị taxi đâm vào. Nếu mọi ô phía trước đều trống, xe taxi sẽ đi đến cuối ảnh chụp nhanh nhất định mà không gặp sự cố, vì vậy đầu ra cần thiết là`You shall pass!!!`. 

Kích thước được cố định: có đúng 3 hàng và đúng 10 cột. Ngay cả việc quét đơn giản từng ô cũng chỉ thực hiện kiểm tra 30 ký tự, không đáng kể trong giới hạn thời gian 1 giây và giới hạn bộ nhớ 256 MB. Không có tham số đầu vào lớn nào có thể khiến việc quét bậc hai hoặc thậm chí tuyến tính gặp vấn đề. Việc tối ưu hóa hữu ích ở đây không cần thiết cho hiệu suất, nhưng nó làm cho cấu trúc của vấn đề trở nên rõ ràng: một khi đã biết hàng của xe taxi thì hai hàng còn lại không liên quan. 

Một trường hợp thường gặp là xe taxi có thể ở cột đầu tiên. Ví dụ:```
=.........
..........
..........
```Đầu ra đúng là`You shall pass!!!`. Việc triển khai bất cẩn bắt đầu kiểm tra từ cột 1 thay vì cột 0 sau khi định vị taxi sẽ xử lý chính xác trường hợp cụ thể này, nhưng mã giả định phải có ít nhất một ô trước khi taxi có thể dễ dàng đưa ra chỉ mục không hợp lệ hoặc bỏ qua quá trình quét hoàn toàn. 

Một trường hợp khác là chướng ngại vật ngay cạnh xe taxi:```
..........
=P........
..........
```Câu trả lời là`P`. Chướng ngại vật phải được kiểm tra bắt đầu từ cột tiếp theo. Bắt đầu quét hai vị trí phía trước sẽ báo cáo không chính xác rằng xe taxi đã đi qua. 

Chướng ngại vật đầu tiên cũng quan trọng khi có nhiều chướng ngại vật xuất hiện trên cùng một làn đường:```
..........
=..H..TP..
..........
```Đầu ra đúng là`H`. Chiếc taxi đâm vào chướng ngại vật đầu tiên nó gặp phải, vì vậy sau đó`T`Và`P`các ô không bao giờ được thay thế câu trả lời trước đó. 

Cuối cùng, chướng ngại vật ở các làn đường khác phải được bỏ qua. Ví dụ:```
..H.......
=.........
.......T..
```Câu trả lời là`You shall pass!!!`. Quét toàn bộ lưới để tìm bất kỳ`H`,`T`, hoặc`P`mà không giới hạn việc tìm kiếm ở hàng taxi sẽ cho kết quả sai. 

## Phương pháp tiếp cận 

Giải pháp brute-force có thể kiểm tra toàn bộ 30 ô, tìm ra vị trí của`=`, rồi kiểm tra các ô đường ở bên phải của hàng đó. Tính đúng đắn của nó xuất phát trực tiếp từ quy tắc di chuyển: xe taxi chỉ có thể ghé thăm các ô trong hàng của nó và nó đi thăm các ô đó từ trái sang phải. Trong trường hợp xấu nhất, nó thực hiện tối đa 30 lần kiểm tra, tiếp theo là tối đa 10 lần kiểm tra dọc theo hàng taxi. Đó là tối đa 40 thao tác liên tục đối với kích thước đầu vào cố định này, vì vậy nó đã nhanh chóng một cách thoải mái. 

Không có điểm thực tế nào mà tại đó phương pháp cưỡng bức này trở nên quá chậm đối với bài toán đã nêu, bởi vì kích thước đường không bao giờ tăng. Nếu ý tưởng tương tự được khái quát hóa thành một con đường có`n`cột, việc quét toàn bộ lưới sẽ mất`O(n)`đối với một số hàng cố định và chỉ quét hàng của xe taxi cũng sẽ mất`O(n)`. Sự khác biệt chủ yếu ở đây là về mặt khái niệm chứ không phải ở vấn đề hiệu suất. 

Quan sát quan trọng là đường đi trong tương lai của xe taxi hoàn toàn được xác định khi chúng ta biết hàng chứa`=`. Hai hàng còn lại không thể ảnh hưởng đến va chạm và các ô bên trái xe taxi đã được vượt qua. Chúng tôi có thể xác định vị trí`=`và sau đó chỉ quét về phía trước dọc theo hàng đó. Nhân vật đầu tiên trong số`H`,`T`, Và`P`ngay lập tức là câu trả lời. 

Do đó, cách thực hiện đơn giản nhất được chấp nhận cũng là cách thực hiện trực tiếp nhất: tìm taxi, đi về bên phải và dừng ở chướng ngại vật đầu tiên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(30), thực tế là O(1) | O(1) | Đã chấp nhận | 
| Tối ưu | O(10), thực tế là O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc 3 hàng đường thành mảng. Chúng tôi giữ lưới vì hàng taxi không được biết trước. 
2. Tìm kiếm ký tự trong ba hàng`=`và ghi lại hàng và cột của nó. Có chính xác một vị trí taxi, vì vậy vị trí này xác định duy nhất con đường quan trọng. 
3. Bắt đầu từ cột ngay sau xe taxi, kiểm tra từng ô một trong khi vẫn ở cùng một hàng. Quá trình quét bắt đầu lúc`taxi_column + 1`bởi vì ô riêng của taxi không phải là chướng ngại vật và ô trước đó không thể tiếp cận được nữa. 
4. Nếu ô hiện tại là`H`,`T`, hoặc`P`, in ký tự đó và dừng lại. Vì quá trình quét di chuyển từ trái sang phải nên đây chắc chắn là trở ngại đầu tiên mà taxi gặp phải. 
5. Nếu quét đến cột 9 mà không tìm thấy chướng ngại vật, hãy in`You shall pass!!!`. Tại thời điểm đó, mọi ô có thể truy cập trong ảnh chụp nhanh đều đã được kiểm tra và trống. 

### Tại sao nó hoạt động 

Bất biến trung tâm là trước khi kiểm tra cột`c`, mọi ô có thể tiếp cận giữa vị trí ban đầu của taxi và cột`c - 1`đã được kiểm tra và không có trở ngại. Xe taxi chỉ di chuyển về bên phải trong hàng ban đầu của nó, vì vậy không có ô nào bị bỏ chọn ngoài chuỗi này có thể gây ra va chạm sớm hơn. Khi quá trình quét tìm thấy chướng ngại vật, bất biến chứng minh rằng không có chướng ngại vật nào xảy ra trước nó, khiến nhân vật đó gặp sự cố đầu tiên. Nếu quá trình quét kết thúc mà không tìm thấy, mọi ô trong đường đi còn lại của xe taxi đều trống, do đó xe taxi không gặp sự cố trong ảnh chụp nhanh. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    grid = [input().strip() for _ in range(3)]

    taxi_row = -1
    taxi_col = -1

    for r in range(3):
        for c in range(10):
            if grid[r][c] == '=':
                taxi_row = r
                taxi_col = c
                break
        if taxi_row != -1:
            break

    for c in range(taxi_col + 1, 10):
        if grid[taxi_row][c] in "HTP":
            print(grid[taxi_row][c])
            return

    print("You shall pass!!!")

if __name__ == "__main__":
    solve()
```Phần đầu vào đọc chính xác ba hàng, khớp với kích thước đường cố định.`strip()`xóa dòng mới mà không thay đổi bất kỳ ký tự đường có ý nghĩa nào. 

Tìm kiếm lồng nhau tìm thấy`=`và lưu trữ cả hàng và cột của nó. thứ hai`break`là cần thiết vì việc tìm taxi kết thúc việc tìm kiếm trên tất cả các hàng chứ không chỉ hàng hiện tại. 

Quá trình quét va chạm bắt đầu lúc`taxi_col + 1`. Ranh giới này là chi tiết lập chỉ mục quan trọng nhất trong quá trình triển khai. Phòng riêng của taxi là`=`, nên kiểm tra sẽ không bao giờ tìm thấy chướng ngại vật, nhưng xuất phát từ sai cột sau có thể bỏ qua chướng ngại vật ngay trước xe taxi. 

Bài kiểm tra thành viên`in "HTP"`nhận ra cả ba trở ngại có thể xảy ra mà không cần ba sự so sánh riêng biệt. Ngay sau khi tìm thấy, ký tự sẽ được in và hàm trả về, giữ nguyên yêu cầu báo cáo chướng ngại vật đầu tiên thay vì chướng ngại vật cuối cùng. 

Không sử dụng số học số nguyên liên quan đến giá trị lớn nên không thể xảy ra tràn. Bản thân lưới chỉ chứa 30 ký tự, giúp việc sử dụng bộ nhớ trở nên ổn định một cách hiệu quả. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Xe taxi ở hàng 1, cột 0 sử dụng chỉ mục dựa trên số 0. Mọi ô bên phải đều trống. 

| Bước | Hàng taxi | Cột taxi | Cột hiện tại | Ô hiện tại | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| Xác định vị trí taxi | 1 | 0 | 0 |`=`| Tìm thấy taxi | 
| Quét | 1 | 0 | 1 |`.`| Tiếp tục | 
| Quét | 1 | 0 | 2 |`.`| Tiếp tục | 
| Quét | 1 | 0 | 3 |`.`| Tiếp tục | 
| Quét | 1 | 0 | 4 |`.`| Tiếp tục | 
| Quét | 1 | 0 | 5 |`.`| Tiếp tục | 
| Quét | 1 | 0 | 6 |`.`| Tiếp tục | 
| Quét | 1 | 0 | 7 |`.`| Tiếp tục | 
| Quét | 1 | 0 | 8 |`.`| Tiếp tục | 
| Quét | 1 | 0 | 9 |`.`| Tiếp tục | 
| Kết thúc | 1 | 0 | 10 | Kết thúc |`You shall pass!!!`| 

Điều này thể hiện trường hợp toàn bộ phần đường có thể tiếp cận bị bỏ trống. Quá trình quét không bao giờ vi phạm tính bất biến của nó vì mọi ô được truy cập đều được xác nhận là trống trước khi di chuyển sang phải xa hơn. 

### Mẫu 2 

Chiếc taxi lại ở hàng 1, cột 0, nhưng`H`xuất hiện ở cột cuối cùng. 

| Bước | Hàng taxi | Cột taxi | Cột hiện tại | Ô hiện tại | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| Xác định vị trí taxi | 1 | 0 | 0 |`=`| Tìm thấy taxi | 
| Quét | 1 | 0 | 1 |`.`| Tiếp tục | 
| Quét | 1 | 0 | 2 |`.`| Tiếp tục | 
| Quét | 1 | 0 | 3 |`.`| Tiếp tục | 
| Quét | 1 | 0 | 4 |`.`| Tiếp tục | 
| Quét | 1 | 0 | 5 |`.`| Tiếp tục | 
| Quét | 1 | 0 | 6 |`.`| Tiếp tục | 
| Quét | 1 | 0 | 7 |`.`| Tiếp tục | 
| Quét | 1 | 0 | 8 |`.`| Tiếp tục | 
| Quét | 1 | 0 | 9 |`H`| In`H`| 

các`H`nằm ở ranh giới của đường, nhưng vẫn có thể tiếp cận được vì cuối cùng xe taxi cũng đến được cột 9. Thuật toán kiểm tra toàn bộ phạm vi từ ô ngay sau xe taxi qua cột cuối cùng, do đó các chướng ngại vật ở ranh giới được xử lý một cách tự nhiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Lưới có chính xác 30 ô và quá trình quét sẽ kiểm tra tối đa 10 ô sau khi định vị taxi. | 
| Không gian | O(1) | Chỉ có lưới 3 x 10 cố định và một vài biến số nguyên được lưu trữ. | 

Bởi vì các kích thước được cố định bởi bài toán nên công việc thực tế bị giới hạn bởi một hằng số nhỏ. Giải pháp này thấp hơn nhiều so với giới hạn thời gian 1 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline
    grid = [input().strip() for _ in range(3)]

    taxi_row = -1
    taxi_col = -1

    for r in range(3):
        for c in range(10):
            if grid[r][c] == '=':
                taxi_row = r
                taxi_col = c
                break
        if taxi_row != -1:
            break

    for c in range(taxi_col + 1, 10):
        if grid[taxi_row][c] in "HTP":
            print(grid[taxi_row][c])
            return

    print("You shall pass!!!")

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided samples
assert run(
    "..........\n"
    "=.........\n"
    "..........\n"
) == "You shall pass!!!", "sample 1"

assert run(
    "..........\n"
    "=........H\n"
    "..........\n"
) == "H", "sample 2"

assert run(
    "..........\n"
    "=........T\n"
    "..........\n"
) == "T", "sample 3"

# Taxi starts at the final column, so there is no space ahead.
assert run(
    "..........\n"
    ".........=\n"
    "..........\n"
) == "You shall pass!!!", "taxi at right boundary"

# An obstacle immediately in front of the taxi must be detected.
assert run(
    "..........\n"
    "=P........\n"
    "..........\n"
) == "P", "immediate obstacle"

# Multiple obstacles are present, but only the first one matters.
assert run(
    "..........\n"
    "=..H..TP..\n"
    "..........\n"
) == "H", "first obstacle"

# Obstacles in the other lanes must be ignored.
assert run(
    "H.........\n"
    "=.T.......\n"
    "........P.\n"
) == "T", "only taxi lane matters"

# The taxi has a clear path through the entire row.
assert run(
    "..........\n"
    "..........\n"
    "=.........\n"
) == "You shall pass!!!", "clear path from bottom lane"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`.........=`ở hàng taxi |`You shall pass!!!`| Taxi đi đúng ranh giới và đường phía trước trống | 
|`=P........`|`P`| Chướng ngại vật liền kề với xe taxi | 
|`=..H..TP..`|`H`| Đặt hàng chướng ngại vật đầu tiên | 
| Chướng ngại vật ở cả ba hàng, taxi ở giữa | Chướng ngại vật ở hàng taxi | Bỏ qua làn đường không liên quan | 
|`=.........`|`You shall pass!!!`| Đường dẫn có thể truy cập hoàn toàn rõ ràng | 

## Vỏ cạnh 

Khi taxi đã ở cột cuối cùng thì phía trước không còn ô nào để kiểm tra. Ví dụ:```
..........
.........=
..........
```Thuật toán tìm`taxi_col = 9`và bắt đầu quét ở cột 10. Vì phạm vi trống nên nó ngay lập tức in`You shall pass!!!`. Điều này tránh việc truy cập ngoài giới hạn và mô hình chính xác thực tế là xe taxi không còn đường nào trong ảnh chụp nhanh. 

Khi có chướng ngại vật ở ngay phía trước, quá trình quét phải bao gồm ô liền kề đó. Vì:```
..........
=P........
..........
```xe taxi ở cột 0 nên quá trình quét bắt đầu ở cột 1. Ô đó chứa`P`, và thuật toán in ngay lập tức`P`. Không có cơ hội để trở ngại sau này ghi đè lên câu trả lời. 

Khi một số chướng ngại vật xảy ra liên tiếp, chướng ngại vật đầu tiên sẽ quyết định kết quả. Vì:```
..........
=..H..TP..
..........
```quá trình quét kiểm tra cột 1 và 2, sau đó đạt tới`H`ở cột 3. Nó in`H`và quay trở lại trước khi đạt được`T`hoặc`P`. Thứ tự quét từ trái sang phải thực thi trực tiếp thứ tự xung đột được yêu cầu. 

Khi các làn đường khác có chướng ngại vật thì chúng không có tác dụng. Coi như:```
H.........
=.........
........P.
```Chiếc taxi ở hàng giữa, và mỗi ô sau`=`ở hàng đó trống. các`H`Và`P`thuộc về các hàng khác nhau nên chúng không bao giờ được kiểm tra trong quá trình quét va chạm. Kết quả là`You shall pass!!!`. 

Đường có kích thước cố định nên trường hợp có ý nghĩa nhỏ nhất vẫn là lưới 3 x 10 bắt buộc. Đường thông thoáng như:```
..........
..........
=.........
```không có chướng ngại vật trên đường đi của taxi. Thuật toán định vị chiếc taxi ở hàng dưới cùng, kiểm tra tất cả chín ô phía trước và in ra`You shall pass!!!`. 

Trường hợp ranh giới cuối cùng là chướng ngại vật ở cột 9, vị trí có thể tiếp cận cuối cùng:```
..........
=........H
..........
```Quá trình quét đến cột 9 sau khi kiểm tra từng ô trước đó và phát hiện`H`. Điều này phát hiện ra lỗi thường gặp khi vòng lặp vô tình dừng ở cột 8 thay vì bao gồm cột cuối cùng.
