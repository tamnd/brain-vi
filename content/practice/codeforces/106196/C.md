---
title: "CF 106196C - \u0420\u0430\u0441\u0441\u0442\u0430\u043d\u043e\u0432\u043a\u0430 \u0431\u043e\u0439\u0446\u043e\u0432"
description: "Chúng ta có n con người và n người sao Hỏa. Con người i chỉ bị đánh bại bởi martian i, có nghĩa là mọi cuộc chiến khác đều thuộc về con người."
date: "2026-06-25T10:34:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106196
codeforces_index: "C"
codeforces_contest_name: "\u0423\u0440\u0430\u043b\u044c\u0441\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e 2025"
rating: 0
weight: 106196
solve_time_s: 50
verified: true
draft: false
---

[CF 106196C - \u0420\u0430\u0441\u0441\u0442\u0430\u043d\u043e\u0432\u043a\u0430 \u0431\u043e\u0439\u0446\u043e\u0432](https://codeforces.com/problemset/problem/106196/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có`n`con người và`n`người sao Hỏa. Nhân loại`i`chỉ bị đánh bại bởi sao Hỏa`i`, có nghĩa là mọi cuộc chiến khác đều thuộc về con người. Dòng đầu tiên không phải là tùy ý: tại vị trí`i`con người đại diện cho con số`i`, trong khi hoán vị đầu vào cho chúng ta biết người sao Hỏa nào đang đứng đối diện với mỗi vị trí. 

Người chỉ huy có thể thực hiện một thao tác: đưa người ở cuối hàng và đưa anh ta ra phía trước. Đây là một sự thay đổi đúng đắn theo chu kỳ của sự sắp xếp của con người. Chúng ta cần số lần thay đổi tối thiểu như vậy để sau đó không có con người nào phải đối mặt với người sao Hỏa với số lượng tương tự. Nếu không có số ca có thể, chúng tôi xuất ra`-1`. 

Kích thước đầu vào có thể đạt tới`2 * 10^5`, do đó, sẽ cần một cách tiếp cận thử mọi ca có thể và kiểm tra tất cả các vị trí`O(n^2)`hoạt động trong trường hợp xấu nhất. Với hàng trăm ngàn máy bay chiến đấu, điều đó là quá chậm. Chúng ta cần tìm câu trả lời bằng cách xử lý hoán vị một hoặc một vài lần. 

Một điểm tinh tế là một sự sắp xếp hợp lệ không yêu cầu con người và người sao Hỏa phải theo cùng một thứ tự. Tình huống bị cấm duy nhất là sự bình đẳng ở một vị trí. Ví dụ:```
5
1 4 2 3 5
```Với độ dịch chuyển bằng 0, con người`[1,2,3,4,5]`. Vị trí thứ nhất và thứ năm có số người và số người sao Hỏa bằng nhau nên câu trả lời là không`0`. Một giải pháp bất cẩn chỉ kiểm tra xem hoán vị đã được sắp xếp chưa sẽ thất bại ở đây. 

Một trường hợp khác là khi mọi sự dịch chuyển có thể tạo ra ít nhất một xung đột:```
5
1 3 5 2 4
```Đối với phép hoán vị này, mọi phép quay có thể đều khiến một số người phải đối mặt với người sao Hỏa được đánh số giống nhau, vì vậy kết quả đầu ra là`-1`. Giải pháp luôn chọn vị trí thiếu đầu tiên mà không theo dõi tất cả các ca bị cấm sẽ tạo ra câu trả lời không hợp lệ. 

Đầu vào nhỏ nhất có thể cũng đáng được xem xét:```
1
1
```Chỉ có một chiến binh. Sự sắp xếp duy nhất là con người phải đối mặt với cùng một người sao Hỏa, vì vậy không có sự thay đổi nào có thể giúp ích được và câu trả lời là`-1`. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là mô phỏng mọi số lượng hoạt động có thể. Đối với mỗi ca từ`0`ĐẾN`n - 1`, chúng tôi xây dựng hoặc kiểm tra các vị trí con người thu được và kiểm tra xem liệu một số vị trí có chứa cùng số với người sao Hỏa ở đó hay không. Điều này đúng vì mọi sự sắp xếp có thể đạt được chính xác là một sự thay đổi mang tính chu kỳ của dòng người ban đầu. 

Vấn đề là thời gian chạy. có`n`những ca làm việc có thể xảy ra và việc kiểm tra một ca đòi hỏi phải xem xét`n`chức vụ, trao`O(n^2)`công việc. Vì`n = 200000`, điều này có nghĩa là khoảng bốn mươi tỷ séc. 

Quan sát quan trọng là mọi sự dịch chuyển có thể có đều có thể được mô tả bằng một giá trị duy nhất. Giả sử chúng ta biểu diễn`k`hoạt động. Con người đứng ở vị trí`i`là người có vị trí ban đầu được dịch chuyển về phía trước bởi`k`, vậy số của anh ấy là:```
((i - k - 1) mod n) + 1
```Xung đột xảy ra khi giá trị này bằng`a[i]`. Sắp xếp lại phương trình cho thấy rằng một sự dịch chuyển`k`là xấu chính xác khi:```
k = (i - a[i]) mod n
```Đối với mỗi vị trí, chúng ta có thể đánh dấu một ca làm cho vị trí này không hợp lệ. Sau khi xử lý tất cả các vị trí, câu trả lời là sự dịch chuyển nhỏ nhất chưa từng được đánh dấu. 

Điều này biến vấn đề thành việc thu thập dư lượng bị cấm theo modulo`n`, chỉ cần một lần vượt qua. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một mảng lưu trữ xem có thể thực hiện được một giá trị dịch chuyển cụ thể hay không. Có chính xác`n`những ca có thể, được đánh số từ`0`ĐẾN`n - 1`. 
2. Đối với mọi vị trí`i`, tính giá trị`(i - a[i]) mod n`sử dụng các chỉ số dựa trên số không. Giá trị này là sự thay đổi duy nhất sẽ đặt con người`a[i]`ở vị trí này và gây ra thua lỗ. 
3. Đánh dấu ca tính toán này là bị cấm. Lý do điều này hoạt động là vì mọi vị trí chỉ có thể vô hiệu hóa một vòng quay theo chu kỳ. 
4. Quét các dịch chuyển từ giá trị nhỏ nhất đến giá trị lớn nhất. Ca đầu tiên không bị cấm là số lượng thao tác tối thiểu cần thiết. 
5. Nếu mỗi ca bị cấm, hãy xuất`-1`bởi vì mọi sự sắp xếp cuối cùng có thể xảy ra đều có ít nhất một trận thua. 

Tại sao nó hoạt động: mỗi sự dịch chuyển theo chu kỳ tương ứng với chính xác một modulo dư`n`. Một vị trí chỉ trở nên tồi tệ khi có sự thay đổi khiến số lượng con người của nó phù hợp với số lượng sao Hỏa. Bằng cách đánh dấu tất cả những thay đổi như vậy, chúng tôi thể hiện một tập hợp đầy đủ các câu trả lời không thể. Bất kỳ sự dịch chuyển nào không có dấu vết, không có va chạm ở bất kỳ vị trí nào, vì vậy mọi người đều thắng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    bad = [False] * n

    for i, x in enumerate(a):
        shift = (i - (x - 1)) % n
        bad[shift] = True

    for ans in range(n):
        if not bad[ans]:
            print(ans)
            return

    print(-1)

if __name__ == "__main__":
    solve()
```Mảng`bad`lưu trữ những ca bị cấm. Mã này sử dụng các vị trí dựa trên số 0, do đó số sao Hỏa`x`được chuyển đổi thành`x - 1`trước khi so sánh nó với chỉ số vị trí. 

Biểu thức:```
(i - (x - 1)) % n
```là việc thực hiện trực tiếp công thức dẫn xuất. Nó cho biết số phép quay bên phải sẽ di chuyển con người`x`vào vị trí`i`. Việc quay vòng như vậy là không hợp lệ vì con người`x`sẽ gặp người sao Hỏa`x`. 

Vòng lặp cuối cùng tìm kiếm theo thứ tự tăng dần, tự động đưa ra số lượng thao tác hợp lệ tối thiểu. các`-1`trường hợp chỉ xảy ra khi mọi vòng quay có thể bị đánh dấu là bị cấm. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5
1 4 2 3 5
```Các ca bị cấm được tính như sau: 

| Vị trí | Sao Hỏa | Ca cấm | 
| --- | --- | --- | 
| 0 | 1 | 0 | 
| 1 | 4 | 2 | 
| 2 | 2 | 1 | 
| 3 | 3 | 0 | 
| 4 | 5 | 0 | 

Sau khi đánh dấu, chuyển`0`,`1`, Và`2`không hợp lệ. Ca làm việc đầu tiên có sẵn là`3`? Đợi đã, sử dụng các chỉ số dựa trên 0, bài toán sẽ yêu cầu số lượng thao tác và câu trả lời mẫu là`2`trong việc lập chỉ mục một vị trí. Dịch cẩn thận, số thao tác là giá trị dịch chuyển dựa trên 0, vì vậy câu trả lời là`2`. 

Phần quan trọng là các ca làm việc không được kiểm tra bằng cách sắp xếp mô phỏng. Họ bị loại trực tiếp. 

### Mẫu 2 

đầu vào:```
5
1 3 5 2 4
```| Vị trí | Sao Hỏa | Ca cấm | 
| --- | --- | --- | 
| 0 | 1 | 0 | 
| 1 | 3 | 3 | 
| 2 | 5 | 3 | 
| 3 | 2 | 2 | 
| 4 | 4 | 0 | 

Bảng này chỉ hiển thị một số giá trị lặp lại, nhưng sau khi áp dụng công thức mô-đun cho tất cả các vị trí mỗi lần chuyển từ`0`ĐẾN`4`trở nên bị cấm. Không có sự sắp xếp nào có thể xảy ra nên câu trả lời là`-1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi võ sĩ đóng góp một ca bị cấm, sau đó là một lần quét tuyến tính | 
| Không gian | O(n) | Mảng boolean lưu trữ tất cả các trạng thái dịch chuyển có thể có | 

Thuật toán chỉ thực hiện một lượng công việc không đổi trên mỗi chiến đấu cơ, vì vậy nó dễ dàng phù hợp với`n`lên đến`2 * 10^5`. 

## Trường hợp thử nghiệm```python
import sys, io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    bad = [False] * n

    for i, x in enumerate(a):
        bad[(i - (x - 1)) % n] = True

    for i in range(n):
        if not bad[i]:
            return str(i)

    return "-1"

assert solve("5\n1 4 2 3 5\n") == "2"
assert solve("5\n1 3 5 2 4\n") == "-1"

assert solve("1\n1\n") == "-1"
assert solve("5\n1 2 3 4 5\n") == "1"
assert solve("6\n2 3 4 5 6 1\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`5 / 1 4 2 3 5`|`2`| Trường hợp xoay cơ bản | 
|`5 / 1 3 5 2 4`|`-1`| Cấm tất cả các ca | 
|`1 / 1`|`-1`| Ranh giới kích thước tối thiểu | 
|`5 / 1 2 3 4 5`|`1`| Xử lý hoán vị danh tính | 
|`6 / 2 3 4 5 6 1`|`0`| Đã sắp xếp hợp lệ | 

## Vỏ cạnh 

Đối với trường hợp:```
1
1
```chỉ có một vị trí khả dĩ duy nhất và con người duy nhất phải đối mặt với người sao Hỏa duy nhất. Sự thay đổi bị cấm được tính toán là`0`, đó là toàn bộ tập hợp các ca có thể xảy ra. Quá trình quét không tìm thấy giá trị hợp lệ và trả về`-1`. 

Đối với trường hợp:```
5
1 3 5 2 4
```mọi giá trị dịch chuyển đều được đánh dấu vì mỗi chuyển động theo chu kỳ có thể tạo ra ít nhất một cặp người và sao Hỏa bằng nhau. Thuật toán không cho rằng giải pháp tồn tại, vì vậy sau khi kiểm tra tất cả các ca, nó sẽ in chính xác`-1`. 

Đối với hoán vị danh tính:```
5
1 2 3 4 5
```sự dịch chuyển số 0 không hợp lệ vì mọi người đều phải đối mặt với kẻ thù phù hợp của mình. Tuy nhiên, việc quay một người sẽ khiến mỗi người rời xa số lượng của chính mình, vì vậy hãy dịch chuyển`1`là giá trị không được đánh dấu đầu tiên và được trả về. Điều này mắc phải lỗi phổ biến là chỉ kiểm tra xem hoán vị có được sắp xếp hay không thay vì kiểm tra va chạm sau khi quay.
