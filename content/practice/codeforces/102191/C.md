---
title: "CF 102191C - Sắp xếp chỗ ngồi"
description: "Đầu vào mô tả chỗ ngồi hình tròn từ tháng trước. Bản thân mảng cung cấp cho học sinh theo thứ tự theo chiều kim đồng hồ, do đó các vị trí mảng liên tiếp là liền kề và phần tử đầu tiên và cuối cùng cũng liền kề."
date: "2026-08-18T09:09:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102191
codeforces_index: "C"
codeforces_contest_name: "PSUT Coding Marathon 2019"
rating: 0
weight: 102191
solve_time_s: 1118
verified: false
draft: false
---

[CF 102191C - Sắp xếp chỗ ngồi](https://codeforces.com/problemset/problem/102191/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 18 phút 38 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Đầu vào mô tả chỗ ngồi hình tròn từ tháng trước. Bản thân mảng cung cấp cho học sinh theo thứ tự theo chiều kim đồng hồ, do đó các vị trí mảng liên tiếp là liền kề và phần tử đầu tiên và cuối cùng cũng liền kề. 

Chúng ta cần xuất ra một thứ tự vòng tròn khác chứa mỗi học sinh đúng một lần. Đối với mỗi cặp học sinh ngồi cạnh nhau trong vòng tròn mới thì cặp đó không được đứng cạnh nhau trong vòng tròn cũ. Mọi thỏa thuận hợp lệ đều được chấp nhận và`-1`là cần thiết khi không có sự sắp xếp như vậy. Việc xây dựng dưới đây tuân theo giải pháp tiêu chuẩn cho vấn đề. 

Với`n`lớn như`3 * 10^5`và chỉ có một giây, giải pháp cần phải tuyến tính hoặc gần tuyến tính. Bất cứ điều gì liên quan đến tất cả các hoán vị đều là không thể ngay lập tức, và thậm chí một`O(n^2)`tìm kiếm sẽ thực hiện xung quanh`9 * 10^10`hoạt động ở giới hạn trên. Cấu trúc hữu ích là bản thân sự sắp xếp cũ là một chu trình, vì vậy chúng ta có thể suy luận về các vị trí thay vì ID sinh viên. 

Có hai giá trị nhỏ cần xử lý đặc biệt. Vì`n = 3`, mọi cặp học sinh đều kề nhau trong vòng tròn cũ nên không có cặp nào cho cạnh mới. Vì`n = 4`, các cặp không liền kề duy nhất là hai cặp học sinh đối diện nhau, tạo thành hai cạnh rời nhau chứ không phải là một chu trình bốn đỉnh. Như vậy cả hai trường hợp đều không thể xảy ra. 

Ví dụ, với`n = 3`và đầu vào`1 3 2`, vòng tròn cũ chứa cả ba cặp có thể có, vì vậy kết quả đúng là`-1`. Việc triển khai bất cẩn chỉ kiểm tra các vị trí liên tiếp trong mảng tuyến tính và quên mất vị trí kề đầu tiên và cuối cùng có thể chấp nhận một sự sắp xếp không chính xác. 

Vì`n = 4`, coi như```
41 2 3 4
```Các vùng lân cận mới được phép chỉ`(1,3)`Và`(2,4)`. Sự sắp xếp hình tròn cần bốn cạnh được phép, nhưng hai cạnh được phép này không thể tạo thành một chu trình, do đó kết quả đầu ra đúng lại là`-1`. 

Ngoài ra còn có một vấn đề về ranh giới`n`. Đơn giản chỉ cần lấy tất cả các phần tử ở vị trí chẵn, theo sau là tất cả các phần tử ở vị trí lẻ gần như có tác dụng, nhưng cạnh cuối cùng đến đầu tiên của nó bị cấm. Ví dụ, đối với các vị trí`0,1,2,3,4,5`, thứ tự`0,2,4,1,3,5`kết thúc bằng cạnh`5 -> 0`, đó là một sự kề cận ban đầu. Hai yếu tố cuối cùng phải được hoán đổi để sửa chữa ranh giới đó. 

Đầu vào được đảm bảo là một hoán vị, do đó phép thử "tất cả các giá trị bằng nhau" không phải là một trường hợp vấn đề hợp lệ. Một bài kiểm tra như`4 / 1 1 1 1`vi phạm hợp đồng đầu vào và không được sử dụng làm bài kiểm tra tính chính xác cho chương trình đã gửi. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử mọi hoán vị của học sinh và kiểm tra xem các cặp liền kề hình tròn của nó có khác với các cặp liền kề hình tròn ban đầu hay không. Một ứng viên duy nhất cần`n`kiểm tra lân cận, trong khi có`n!`ứng viên, từ bỏ`n * n!`kiểm tra trong trường hợp xấu nhất. Ngay cả đối với`n = 10`, đó là về`36.3`triệu lượt kiểm tra lân cận. Tại`n = 3 * 10^5`, tăng trưởng giai thừa làm cho phương pháp này hoàn toàn không thể sử dụng được. 

Lực lượng vũ phu có tác dụng vì việc kiểm tra ứng viên trực tiếp rất dễ dàng. Khó khăn là tìm được ứng viên. Quan sát hữu ích là giới hạn chỉ phụ thuộc vào các vị trí trong vòng tròn cũ. Nếu hai vị trí cũ khác nhau không`1`cũng không`-1`modulo`n`, học sinh của họ được an toàn khi xếp cạnh nhau. 

Điều này gợi ý nên đảm nhận các vị thế có bước cố định lớn hơn. Đối với số lẻ`n`, bước ngang qua`2`modulo`n`ghé thăm mọi vị trí đúng một lần vì`2`Và`n`là nguyên tố cùng nhau. Mỗi cặp liên tiếp theo thứ tự mới này được phân tách bằng hai vị trí trong vòng tròn cũ, bao gồm cả cặp cuối cùng, do đó mọi kề cận mới đều hợp lệ. 

Thậm chí`n`, bước ngang qua`2`chỉ truy cập các vị trí có cùng tính chẵn lẻ. Đầu tiên chúng ta có thể đặt tất cả các vị trí chẵn vào đáp án và sau đó là tất cả các vị trí lẻ. Bên trong mỗi nhóm, các vị trí liên tiếp khác nhau hai trong vòng tròn ban đầu, vì vậy các cạnh đó đều an toàn. Chỉ có vấn đề ở ranh giới. Hoán đổi hai phần tử cuối cùng của nhóm vị trí lẻ sẽ thay đổi cả hai cạnh biên thành các phần kề không phải là gốc khi`n >= 6`. 

Điều này mang lại một sự đơn giản`O(n)`sự thi công. Thực tế là không có câu trả lời nào cho`n < 5`và rằng công trình xây dựng phù hợp với mọi`n >= 5`cũng được phản ánh trong việc triển khai tham chiếu đã biết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n · n!)`|`O(n)`| Quá chậm | 
| Tối ưu |`O(n)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hoán vị vòng tròn`a`. Chúng ta sẽ xây dựng câu trả lời bằng cách sử dụng các vị trí trong`a`, bởi vì mối quan hệ bị cấm đoán được xác định hoàn toàn bởi quan điểm cũ. 
2. Nếu`n < 5`, in`-1`. Vì`n = 3`Và`n = 4`, phần bù của chu trình cũ không chứa chu trình Hamilton nên không có cách sắp xếp vòng tròn nào có thể thỏa mãn mọi ràng buộc. 
3. Nối thêm`a[0], a[2], a[4], ...`để trả lời. Các vị trí này cách nhau hai vị trí trong vòng tròn ban đầu, vì vậy mọi phần kề được tạo bên trong phần này đều được cho phép. 
4. Nối thêm`a[1], a[3], a[5], ...`để trả lời. Lập luận tương tự cũng áp dụng cho phần thứ hai này, vì các vị trí liên tiếp của nó cũng khác nhau hai điểm. 
5. Nếu`n`thật kỳ lạ, dừng lại ở đây. Trình tự chính xác là thứ tự thu được bằng cách di chuyển liên tục hai vị trí xung quanh vòng tròn ban đầu. Bởi vì`gcd(2, n) = 1`, điều này sẽ ghé thăm mọi vị trí một lần và cặp từ cuối đến đầu tiên cũng cách nhau hai vị trí theo modulo`n`. 
6. Nếu`n`chẵn, hoán đổi hai phần tử cuối cùng của câu trả lời đã xây dựng. Trước khi hoán đổi, phần tử cuối cùng là`a[n-1]`, sẽ liền kề với phần tử đầu tiên`a[0]`trong vòng tròn cũ. Sau khi hoán đổi, phần tử cuối cùng sẽ trở thành`a[n-3]`, được tách biệt an toàn khỏi`a[0]`khi`n >= 6`. Ranh giới bị ảnh hưởng khác cũng trở nên an toàn. 
7. In hoán vị kết quả. Nó chứa mọi phần tử ban đầu chính xác một lần vì chúng tôi chỉ sắp xếp lại các vị trí ban đầu. 

### Tại sao nó hoạt động 

Xét số lẻ đầu tiên`n`. Mỗi cạnh trả lời kết nối các vị trí có chênh lệch là`2`modulo`n`. Từ`n >= 5`, sự khác biệt của`2`không phải là một vùng lân cận cũ, nơi mà những khác biệt duy nhất có thể có là`1`Và`n - 1`. Bởi vì`2`là số nguyên tố cùng với số lẻ`n`, thêm liên tục`2`thăm tất cả các vị trí, do đó việc xây dựng là một hoán vị vòng tròn. 

Thậm chí`n >= 6`, tất cả các cạnh bên trong nhóm vị trí chẵn và bên trong nhóm vị trí lẻ có sự khác biệt về vị trí`2`. Các cạnh nguy hiểm tiềm tàng duy nhất là hai kết nối giữa các nhóm và cạnh cuối cùng đến đầu tiên. Sau khi hoán đổi hai phần tử cuối cùng, các vị trí kết nối này`n-2`ĐẾN`1`,`n-3`ĐẾN`n-1`, Và`n-3`ĐẾN`0`. Khoảng cách vòng tròn của họ ít nhất là`3`hoặc`2`mà không có`1`hoặc`n-1`, vì vậy không có gì là một vùng lân cận cũ. Do đó mọi cạnh trong đường tròn được tạo ra đều hợp lệ. 

Điều bất biến là mỗi cặp được đặt liên tiếp đều cách nhau hai vị trí trong vòng tròn ban đầu hoặc là một trong những ranh giới giữa các nhóm được sửa chữa đặc biệt. Không có cặp nào trong số đó là cặp lân cận ban đầu. 

## Giải pháp Python```python
Pythonimport sysinput = sys.stdin.readline

def solve():    n = int(input())    a = list(map(int, input().split()))
    if n < 5:        print(-1)        return
    ans = []
    for i in range(0, n, 2):        ans.append(a[i])
    for i in range(1, n, 2):        ans.append(a[i])
    if n % 2 == 0:        ans[-1], ans[-2] = ans[-2], ans[-1]
    print(*ans)

if __name__ == "__main__":    solve()
```Vòng lặp đầu tiên lấy mọi chỉ mục chẵn, trong khi vòng lặp thứ hai lấy mọi chỉ mục lẻ. Chúng cùng nhau chứa mọi vị trí chính xác một lần, do đó không có học sinh nào bị mất hoặc trùng lặp. 

Đối với số lẻ`n`, không có gì khác là cần thiết. Trình tự kết quả là bước đi ngang thứ hai của đường tròn ban đầu. Ví dụ: với năm vị trí, thứ tự vị trí là`0, 2, 4, 1, 3`và cạnh đóng đi từ`3`quay lại`0`, lại một khoảng cách bằng hai xung quanh vòng tròn. 

Thậm chí`n`, riêng hai nhóm rời khỏi vị trí ban đầu cuối cùng. Từ`a[n-1]`đã ở gần`a[0]`, chỉ cần đóng vòng tròn mới sẽ tạo ra chính xác cạnh cấm mà chúng ta đang cố tránh. Hoán đổi`ans[-1]`Và`ans[-2]`di chuyển`a[n-3]`đến vị trí cuối cùng và đặt`a[n-1]`ngay trước đó`a[n-3]`. Cả hai cạnh kết quả đều hợp lệ cho`n >= 6`. 

Không có vấn đề tràn số nguyên vì thuật toán chỉ lưu trữ và lập chỉ mục các số nguyên từ đầu vào. Việc triển khai cũng tránh mọi tìm kiếm tốn kém, do đó thời gian chạy của nó bị chi phối bởi việc đọc và in`n`các giá trị. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
86 1 3 5 7 8 4 2
```Những thay đổi trạng thái quan trọng là: 

| Bước | Hoạt động | Trả lời | 
| --- | --- | --- | 
| 1 | Lấy chỉ số chẵn`0,2,4,6`|`6 3 7 4`| 
| 2 | Lấy chỉ số lẻ`1,3,5,7`|`6 3 7 4 1 5 8 2`| 
| 3 |`n`chẵn, hoán đổi hai cái cuối cùng |`6 3 7 4 1 5 2 8`| 

Sự sắp xếp cuối cùng là`6 3 7 4 1 5 2 8`. Các vị trí cũ liên tiếp của nó được tách biệt an toàn, kể cả cạnh tròn từ`8`quay lại`6`. Mẫu có nhiều câu trả lời hợp lệ, do đó, mẫu này khác với kết quả mẫu của câu lệnh nhưng có giá trị như nhau. 

### Mẫu 2 

Đầu vào là```
31 3 2
```Thuật toán dừng ngay lập tức: 

| Bước | Tình trạng | Kết quả | 
| --- | --- | --- | 
| 1 |`n = 3`|`n < 5`| 
| 2 | Không có nỗ lực xây dựng |`-1`| 

Với ba học sinh, vòng tròn cũ đã chứa mọi cặp có thể là liền kề. Vòng tròn mới không có lợi thế pháp lý nên việc từ chối là không thể tránh khỏi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n)`| Mỗi phần tử đầu vào được xử lý một lần và câu trả lời được in một lần. | 
| Không gian |`O(n)`| Mỗi mảng đầu vào và câu trả lời được xây dựng đều chứa`n`các phần tử. | 

Vì`n <= 3 * 10^5`, công việc tuyến tính dễ dàng phù hợp với giới hạn một giây trong Python. Việc sử dụng bộ nhớ cũng thoải mái dưới 256 MB vì ​​chỉ có hai mảng kích thước`n`được duy trì. 

## Trường hợp thử nghiệm 

Vì nhiều đầu ra hợp lệ nên bộ khai thác thử nghiệm sẽ xác thực hoán vị được tạo ra thay vì so sánh nó với một đầu ra cố định. Người trợ giúp bên dưới kiểm tra xem đầu ra có phải là`-1`đối với trường hợp không thể xảy ra hoặc một hoán vị hợp lệ có các cặp hình tròn liền kề không liền kề trong hình tròn ban đầu.```python
Pythonimport sysimport io

def solve_data(inp: str) -> str:    data = inp.split()    n = int(data[0])    a = list(map(int, data[1:]))
    if n < 5:        return "-1"
    ans = []
    for i in range(0, n, 2):        ans.append(a[i])
    for i in range(1, n, 2):        ans.append(a[i])
    if n % 2 == 0:        ans[-1], ans[-2] = ans[-2], ans[-1]
    return " ".join(map(str, ans))

def run(inp: str) -> str:    return solve_data(inp)

def valid(inp: str, out: str) -> bool:    data = inp.split()    n = int(data[0])    a = list(map(int, data[1:]))
    if out.strip() == "-1":
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 / 1 3 2`|`-1`| Trường hợp không thể có kích thước tối thiểu | 
|`4 / 1 2 3 4`|`-1`| Kích thước không thể khác | 
|`5 / 1 2 3 4 5`| Bất kỳ hoán vị hợp lệ nào | Trường hợp nhỏ nhất có thể giải được và cách xây dựng kỳ lạ | 
|`6 / 1 2 3 4 5 6`| Bất kỳ hoán vị hợp lệ nào | Ngay cả việc xây dựng và trao đổi cuối cùng | 
|`300000 / 1 2 ... 300000`| Bất kỳ hoán vị hợp lệ nào | Hạn chế tối đa và hiệu suất tuyến tính | 

Trường hợp hoàn toàn bằng nhau được yêu cầu trong mô tả kiểm tra không thể là đầu vào hợp lệ vì vấn đề rõ ràng yêu cầu hoán vị của`1`bởi vì`n`. Kiểm thử nó sẽ kiểm tra hành vi bên ngoài đặc tả hơn là kiểm tra thuật toán. 

## Vỏ cạnh 

cho`n = 3`, đầu vào```
31 3 2
```làm cho thuật toán trả về`-1`trước khi xây dựng bất cứ điều gì. Mọi cặp trong số ba học sinh đều đã kề nhau trong đường tròn cũ nên không có cạnh hình tròn mới nào có thể được hình thành một cách hợp pháp. 

Vì`n = 4`, coi như```
41 2 3 4
```Việc xây dựng cũng bị từ chối ngay lập tức. Sinh viên`1`Và`3`ngược lại, cũng như`2`Và`4`, và đó là những cặp hợp pháp duy nhất. Họ không thể cung cấp bốn cạnh cần thiết cho một vòng tròn bốn người. 

Đối với trường hợp lẻ nhỏ nhất có thể giải được,```
51 2 3 4 5
```đường chuyền vị trí chẵn tạo ra`1 3 5`và việc chuyển vị trí lẻ tạo ra`2 4`, cho`1 3 5 2 4`. Mỗi cặp lân cận mới tương ứng với bước nhảy hai vị trí trong vòng tròn cũ, bao gồm`4 -> 1`. Điều này chứng tỏ tại sao kỳ lạ`n`không cần chỉnh sửa. 

Đối với trường hợp chẵn nhỏ nhất giải được,```
61 2 3 4 5 6
```việc xây dựng ban đầu là`1 3 5 2 4 6`. Cạnh cuối cùng`6 -> 1`bị cấm vì ban đầu những học sinh đó ở cạnh nhau. Hoán đổi hai phần tử cuối cùng mang lại`1 3 5 2 6 4`. Các cạnh tròn bây giờ kết nối các vị trí ban đầu với khoảng cách`2, 2, 3, 4, 2, 3`, không có cái nào trong số đó là liền kề ban đầu. Đây là trường hợp ranh giới phát hiện các triển khai thực hiện phân chia chẵn lẻ nhưng quên hoán đổi cuối cùng. 

Đối với đầu vào chẵn lớn, lý do tương tự không thay đổi. Việc xây dựng không bao giờ tìm kiếm một sinh viên đặc biệt hoặc liên tục kiểm tra các cạnh được xây dựng trước đó. Nó chỉ tính toán các nhóm chẵn lẻ và thực hiện một lần hoán đổi, do đó tăng`n`từ`6`ĐẾN`300000`thay đổi số lượng công việc tuyến tính nhưng không thay đổi logic.
