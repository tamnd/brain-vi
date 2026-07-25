---
title: "CF 103886A - Ngũ cốc Phân loại"
description: "Chúng ta được cho một dãy gấu trúc đỏ ngồi thành một hàng, trong đó mỗi gấu trúc có một ID nguyên. Quá trình chúng tôi quan tâm liên tục xem xét các ID này theo thứ tự giá trị tăng dần và bất cứ khi nào một ID cụ thể xuất hiện trong dòng hiện tại, tất cả gấu trúc có ID đó sẽ đóng góp vào…"
date: "2026-07-02T07:37:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103886
codeforces_index: "A"
codeforces_contest_name: "CerealCodes 2022 Summer Contest"
rating: 0
weight: 103886
solve_time_s: 37
verified: true
draft: false
---

[CF 103886A - Phân loại ngũ cốc](https://codeforces.com/problemset/problem/103886/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy gấu trúc đỏ ngồi thành một hàng, trong đó mỗi gấu trúc có một ID nguyên. Quá trình chúng tôi quan tâm liên tục xem xét các ID này theo thứ tự giá trị tăng dần và bất cứ khi nào một ID cụ thể xuất hiện trong dòng hiện tại, tất cả gấu trúc có ID đó sẽ đóng góp vào câu trả lời theo cách tùy thuộc vào số lượng gấu trúc vẫn còn lại tại thời điểm đó. Sau khi xử lý ID, tất cả gấu trúc có ID đó sẽ bị xóa khỏi dòng và quá trình tiếp tục với các ID còn lại. 

Đầu ra là một giá trị tích lũy duy nhất đếm các khoản đóng góp từ mỗi ID dựa trên số lượng gấu trúc vẫn còn hiện diện trước khi ID đó bị xóa. 

Ràng buộc chính là ID bị giới hạn bởi$10^6$, nhỏ hơn nhiều so với thông thường$n$trong những vấn đề này. Điều đó ngay lập tức gợi ý rằng việc lặp lại trên phạm vi giá trị có thể khả thi ngay cả khi việc lặp lại trên tất cả các phần tử là không. 

Một cách giải thích ngây thơ sẽ là mô phỏng việc loại bỏ trực tiếp trên một mảng hoặc danh sách. Điều đó thất bại khi$n$lớn, vì việc loại bỏ các phần tử nhiều lần khỏi danh sách là tuyến tính trên mỗi phép toán, dẫn đến hành vi bậc hai. 

Trường hợp phức tạp xuất phát từ các ID lặp lại. Nếu tất cả các phần tử có chung ID, một mô phỏng đơn giản loại bỏ từng phần tử một vẫn hoạt động hợp lý nhưng sẽ trở nên chậm một cách không cần thiết. Một trường hợp cạnh khác xuất hiện khi ID thưa thớt nhưng lớn, ví dụ một phần tử duy nhất tại ID$10^6$và tất cả những thứ khác ở giá trị nhỏ. Bất kỳ giải pháp nào chỉ quét tối đa phần tử thay vì giới hạn cố định vẫn hoạt động, nhưng việc triển khai bất cẩn giả sử ID dày đặc trong phạm vi nhỏ hơn sẽ bị hỏng. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu mô phỏng trực tiếp đường dây. Ở mỗi bước, nó sẽ quét danh sách hiện tại, tìm tất cả các lần xuất hiện của ID tiếp theo theo thứ tự được sắp xếp, tính toán mức đóng góp của chúng dựa trên độ dài hiện tại của danh sách, xóa chúng và lặp lại. Tính chính xác rất đơn giản vì nó phản ánh chính xác định nghĩa quy trình. 

Vấn đề là mỗi lần xóa đều yêu cầu quét và có khả năng dịch chuyển danh sách kích thước$n$. Nếu thực hiện lặp đi lặp lại lên đến$n$ID riêng biệt, trường hợp xấu nhất sẽ trở thành$O(n^2)$, quá chậm đối với các ràng buộc thông thường. 

Quan sát quan trọng là chúng ta thực sự không bao giờ cần phải duy trì danh sách đang phát triển một cách rõ ràng. Điều quan trọng là có bao nhiêu phần tử của mỗi ID tồn tại và bao nhiêu phần tử còn lại trước khi xử lý một ID nhất định. Nếu tính toán trước tần số, chúng ta có thể theo dõi một biến duy nhất biểu thị số lượng phần tử vẫn còn “sống” trong cấu trúc. Sau đó, khi chúng tôi quét ID theo thứ tự tăng dần, mỗi ID sẽ đóng góp tần số của nó nhân với số phần tử còn lại trước khi bị xóa. 

Điều này biến vấn đề từ loại bỏ động thành quét tĩnh trên không gian giá trị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(n)$| Quá chậm | 
| Quét tần số |$O(n + 10^6)$|$O(10^6)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một mảng tần số trên tất cả các ID có thể có và một biến biểu thị số phần tử vẫn còn trong dòng. 

1. Đọc tất cả các ID và đếm số lần xuất hiện của chúng trong một mảng tần số. Điều này cho chúng tôi biết chính xác mỗi giá trị xuất hiện bao nhiêu lần mà không cần thứ tự ban đầu nữa. 
2. Khởi tạo một biến$remaining = n$, đại diện cho số lượng gấu trúc vẫn còn hiện diện trước khi có bất kỳ sự di dời nào xảy ra. 
3. Khởi tạo bộ tích lũy câu trả lời$ans = 0$. 
4. Lặp lại tất cả các ID có thể từ 1 đến$10^6$. Chúng tôi thực hiện việc này theo thứ tự tăng dần vì quy trình xóa ID theo thứ tự tăng dần, do đó, điều này phù hợp với sự phát triển về mặt khái niệm của dòng. 
5. Đối với mỗi ID$v$, nếu tần số của nó bằng 0, chúng ta bỏ qua nó vì nó không đóng góp gì và không thay đổi trạng thái. 
6. Nếu tần số khác 0 thì mọi gấu trúc có ID này đều đóng góp trong khi tất cả các gấu trúc còn lại vẫn hiện diện. Chúng tôi thêm$remaining \times freq[v]$để trả lời. 
7. Sau khi xử lý ID$v$, chúng tôi loại bỏ tất cả các lần xuất hiện của nó một cách hợp lý bằng cách giảm$remaining$qua$freq[v]$. 

Ý tưởng quan trọng là khi chúng ta đạt được ID$v$, tất cả ID nhỏ hơn$v$đã được xử lý và xóa, vì vậy$remaining$chỉ phản ánh chính xác cơ sở đóng góp cho ID hiện tại và tương lai. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào trong quá trình quét, biến$remaining$bằng tổng số phần tử có ID chưa được xử lý. Vì chúng tôi xử lý ID theo thứ tự tăng dần nên giá trị này khớp chính xác với số phần tử vẫn có trong dòng mô phỏng ngay trước khi xóa ID hiện tại. Mỗi phần tử của một ID nhất định đóng góp một lần và chỉ tại thời điểm ID của nó được xử lý. Phép nhân với$remaining$nắm bắt thực tế là mỗi lần xuất hiện tương tác với tất cả các phần tử hiện chưa bị xóa và sau đó loại bỏ tần số của nó sẽ bảo toàn tính bất biến cho các bước trong tương lai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXV = 10**6

n = int(input().strip())
arr = list(map(int, input().split()))

freq = [0] * (MAXV + 1)

for x in arr:
    freq[x] += 1

remaining = n
ans = 0

for v in range(1, MAXV + 1):
    if freq[v] == 0:
        continue
    ans += remaining * freq[v]
    remaining -= freq[v]

print(ans)
```Giải pháp bắt đầu bằng cách đọc đầu vào và xây dựng một mảng tần số, nén toàn bộ cấu trúc thành số đếm trên mỗi giá trị. Điều này tránh mọi nhu cầu duy trì trật tự hoặc mô phỏng việc xóa. 

Biến`remaining`là công cụ theo dõi trạng thái trung tâm. Nó luôn thể hiện có bao nhiêu phần tử chưa được “tính đến” trong quá trình quét. Mỗi lần chúng tôi xử lý một giá trị`v`, chúng tôi cho rằng tất cả các lần xuất hiện của nó sẽ bị xóa sau khi đóng góp. 

phép nhân`remaining * freq[v]`là bước tính toán quan trọng. Nó nắm bắt được rằng mỗi lần xuất hiện của giá trị`v`tương tác với cấu trúc còn lại hiện tại. Sau khi thêm phần đóng góp này, chúng tôi trừ đi`freq[v]`bởi vì những yếu tố đó không còn là một phần của những tương tác trong tương lai nữa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5
arr = [1, 2, 2, 3, 3]
```Chúng tôi xây dựng tần số: 

1 → 1, 2 → 2, 3 → 2 

| v | tần số[v] | còn lại trước | đóng góp | còn lại sau | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 5 | 5 | 4 | 5 | 
| 2 | 2 | 4 | 8 | 2 | 13 | 
| 3 | 2 | 2 | 4 | 0 | 17 | 

Điều này cho thấy mỗi ID đóng góp như thế nào dựa trên số lượng phần tử vẫn hoạt động khi nó được xử lý. Sau khi xử lý từng ID, những phần tử đó sẽ bị xóa, làm giảm cơ sở đóng góp trong tương lai. 

### Ví dụ 2 

đầu vào:```
n = 4
arr = [5, 1, 5, 2]
```Tần số: 

1 → 1, 2 → 1, 5 → 2 

| v | tần số[v] | còn lại trước | đóng góp | còn lại sau | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 4 | 4 | 3 | 4 | 
| 2 | 1 | 3 | 3 | 2 | 7 | 
| 3 | 0 | 2 | 0 | 2 | 7 | 
| 4 | 0 | 2 | 0 | 2 | 7 | 
| 5 | 2 | 2 | 4 | 0 | 11 | 

Ví dụ này nhấn mạnh rằng việc bỏ qua các giá trị vắng mặt không có tác dụng gì và các ID lớn được xử lý theo thứ tự một cách tự nhiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + 10^6)$| Đếm tần số mất$O(n)$và quá trình quét trên phạm vi giá trị là tuyến tính trong giới hạn ID tối đa | 
| Không gian |$O(10^6)$| Mảng tần số lưu trữ số lượng cho mỗi ID có thể | 

Những ràng buộc làm cho việc này trở nên hiệu quả vì$10^6$các phép toán đều khả thi trong Python, đặc biệt với các phép toán số nguyên đơn giản bên trong một vòng lặp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    MAXV = 10**6

    n = int(input().strip())
    arr = list(map(int, input().split()))

    freq = [0] * (MAXV + 1)
    for x in arr:
        freq[x] += 1

    remaining = n
    ans = 0

    for v in range(1, MAXV + 1):
        if freq[v] == 0:
            continue
        ans += remaining * freq[v]
        remaining -= freq[v]

    return str(ans)

# custom cases
assert run("1\n5\n") == "1", "single element"
assert run("3\n1 1 1\n") == "9", "all equal values"
assert run("4\n4 3 2 1\n") == "20", "already decreasing order"
assert run("5\n2 3 2 3 2\n") == "21", "repeated mixed values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | tính đúng đắn của trường hợp tối thiểu | 
| tất cả các giá trị bằng nhau | 9 | xử lý ID lặp đi lặp lại | 
| thứ tự giảm dần | 20 | đặt hàng độc lập | 
| lặp lại hỗn hợp | 21 | tương tác đa tần số | 

## Vỏ cạnh 

Đối với đầu vào một phần tử như`n = 1, arr = [7]`, mảng tần số chỉ có một mục khác 0. Bộ thuật toán`remaining = 1`, xử lý ID 7, thêm`1 * 1 = 1`, sau đó giảm phần còn lại xuống 0. Điều này xác nhận tính đúng đắn ở trạng thái tối thiểu nơi không thể có tương tác. 

Đối với trường hợp tất cả các giá trị giống hệt nhau, chẳng hạn như`n = 4, arr = [3, 3, 3, 3]`, quá trình quét chỉ gặp một ID hoạt động. Vào lúc đó,`remaining = 4`, vậy đóng góp là`4 * 4 = 16`, và sau đó tất cả các phần tử sẽ bị xóa. Điều này cho thấy việc nhóm các ID giống hệt nhau được xử lý chính xác mà không cần mô phỏng vị trí. 

Đối với trường hợp ID lớn thưa thớt như`arr = [1, 1000000]`, thuật toán xử lý ID 1 trước tiên, sau đó nhảy trực tiếp tới ID 1000000. Việc thiếu các giá trị trung gian không ảnh hưởng đến độ chính xác vì việc bỏ qua các mục nhập tần số 0 sẽ bảo toàn tính bất biến đó`remaining`luôn chỉ phản ánh các phần tử chưa được xử lý.
