---
title: "CF 106447A - \u0411\u0438\u0431\u0438 \u0438 \u0435\u0433\u043e \u043f\u0430\u043f\u0430"
description: "Tác vụ mô phỏng việc chèn số vào một bảng băm nhỏ. Bảng có số lượng nhóm cố định và số x được gán cho nhóm x mod p. Mỗi thùng chỉ có thể lưu trữ một số."
date: "2026-06-25T09:23:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106447
codeforces_index: "A"
codeforces_contest_name: "\u041a\u043e\u0433\u043d\u0438\u0442\u0438\u0432\u043d\u044b\u0435 \u0442\u0435\u0445\u043d\u043e\u043b\u043e\u0433\u0438\u0438 2025-2026. \u0424\u0438\u043d\u0430\u043b"
rating: 0
weight: 106447
solve_time_s: 32
verified: true
draft: false
---

[CF 106447A - \u0411\u0438\u0431\u0438 \u0438 \u0435\u0433\u043e \u043f\u0430\u043f\u0430](https://codeforces.com/problemset/problem/106447/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 32s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Tác vụ mô phỏng việc chèn số vào một bảng băm nhỏ. Bảng có một số nhóm cố định và một số`x`được gán cho thùng`x mod p`. Mỗi thùng chỉ có thể lưu trữ một số. Các số lần lượt đến theo thứ tự nhất định và chúng ta phải tìm vị trí đầu tiên nơi phần chèn cố gắng sử dụng một nhóm đã chứa một số khác. Nếu mỗi lần chèn vào một thùng trống, câu trả lời là`-1`. 

Đầu vào cung cấp số lượng nhóm`p`, số lượng giá trị cần chèn`n`và sau đó là chuỗi các giá trị. Đầu ra là chỉ mục dựa trên 1 của lần chèn xung đột đầu tiên. 

Các ràng buộc là nhỏ, với cả hai`p`Và`n`giới hạn ở 300. Điều này có nghĩa là ngay cả một mô phỏng đơn giản cũng đủ nhanh. Số lần chèn chỉ vài trăm, vì vậy các thuật toán có hành vi tuyến tính hoặc bậc hai đều phù hợp. Không cần cấu trúc băm nâng cao vì toàn bộ bảng rất nhỏ. 

Các trường hợp cạnh chính đến từ thứ tự chèn và từ các giá trị trông khác nhau nhưng ánh xạ tới cùng một nhóm. Ví dụ: với:```
5 3
1
6
11
```đầu ra đúng là:```
2
```bởi vì`1 mod 5`,`6 mod 5`, Và`11 mod 5`tất cả đều bằng nhau`1`. Lần chèn thứ hai đã là một cuộc xung đột. Việc triển khai bất cẩn lưu trữ các số gốc thay vì vị trí nhóm sẽ cho rằng cả ba giá trị đều khác nhau một cách không chính xác. 

Một trường hợp khác là khi tất cả các thùng được lấp đầy mà không có xung đột:```
4 4
0
1
2
3
```Đầu ra đúng là:```
-1
```Giải pháp cho rằng lần chèn cuối cùng không thành công sẽ là sai. Một bảng đầy đủ vẫn hợp lệ nếu mọi giá trị được chèn vào một nhóm duy nhất. 

Trường hợp ranh giới cuối cùng là khi cùng một giá trị xuất hiện hai lần:```
10 2
123456789
123456789
```Đầu ra đúng là:```
2
```Giá trị thứ hai ánh xạ tới cùng một nhóm với giá trị đầu tiên. Việc chỉ so sánh xem các giá trị ban đầu có được lặp lại hay không là không cần thiết vì xung đột cũng có thể xảy ra giữa các giá trị khác nhau. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là mô phỏng bảng băm chính xác như được mô tả. Duy trì nội dung hiện tại của mỗi nhóm. Đối với mỗi số đến, hãy tính chỉ số nhóm của nó bằng thao tác còn lại và kiểm tra xem nhóm đó đã được sử dụng chưa. Nếu nó trống, đánh dấu nó là đã sử dụng và tiếp tục. Nếu nó bị chiếm giữ, vị trí hiện tại là câu trả lời. 

Phương pháp này đúng vì bài toán yêu cầu xung đột đầu tiên theo thứ tự chèn thực tế và mô phỏng trực tiếp tuân theo thứ tự đó. Trường hợp xấu nhất thực hiện`n`các lần chèn và một lần kiểm tra liên tục cho mỗi lần chèn, do đó, ngay cả việc triển khai trực tiếp cũng chỉ cần khoảng 300 thao tác. 

Thông tin chi tiết quan trọng là bản thân bảng băm là trạng thái duy nhất quan trọng. Khi một giá trị đã được chuyển đổi thành chỉ mục nhóm của nó, số ban đầu không còn ảnh hưởng đến các quyết định trong tương lai. Chúng tôi hoàn toàn không cần lưu trữ các số, chỉ cần lưu trữ mỗi nhóm đã nhận được một phần tử hay chưa. 

Ở đây, mô phỏng lực lượng vũ phu và giải pháp tối ưu thực sự là cùng một ý tưởng. Việc tối ưu hóa nhận ra rằng trạng thái có thể được biểu diễn bằng một mảng kích thước boolean đơn giản`p`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(p) | Đã chấp nhận | 
| Tối ưu | O(n) | O(p) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một mảng đại diện cho các nhóm của bảng băm. Mỗi vị trí lưu trữ xem nhóm đó đã được sử dụng hay chưa. 
2. Đọc từng số theo thứ tự xuất hiện. Tính chỉ số nhóm của nó như`x mod p`. 
3. Kiểm tra thùng tương ứng. Nếu nó đã bị chiếm dụng, hãy xuất vị trí chèn hiện tại vì đây là lần xung đột đầu tiên. 
4. Nếu thùng trống, hãy đánh dấu nó là đã có người sử dụng và tiếp tục xử lý số tiếp theo. 
5. Nếu tất cả các số được xử lý mà không tìm thấy xung đột, hãy xuất ra`-1`. 

Lý do điều này có tác dụng là vì mỗi lần chèn trong tương lai chỉ phụ thuộc vào nhóm nào được sử dụng chứ không phụ thuộc vào giá trị nào đã tạo ra các nhóm bị chiếm dụng đó. Việc giữ chính xác thông tin này là đủ để tái tạo hoạt động của bảng băm. 

Tại sao nó hoạt động: sau khi xử lý lần đầu tiên`i`các giá trị, mảng nhóm khớp chính xác với bảng băm thực sau`i`phần chèn thêm. Đối với giá trị tiếp theo, thuật toán sẽ kiểm tra cùng một nhóm mà thao tác chèn thực sẽ sử dụng. Nếu nó bị chiếm dụng, cả quy trình thực và thuật toán sẽ phát hiện xung đột ở bước này. Nếu nó trống, cả hai đều đánh dấu nó là đã điền và tiếp tục. Vì bất biến giữ nguyên sau mỗi lần chèn nên xung đột được báo cáo đầu tiên luôn đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    p, n = map(int, input().split())
    used = [False] * p

    for i in range(1, n + 1):
        x = int(input())
        bucket = x % p

        if used[bucket]:
            print(i)
            return

        used[bucket] = True

    print(-1)

if __name__ == "__main__":
    solve()
```Mảng`used`đại diện cho các nhóm bảng băm. Kích thước của nó chính xác là`p`, vì vậy mọi chỉ mục nhóm có thể có đều có vị trí tương ứng. 

Vòng lặp bắt đầu từ`1`vì câu trả lời dựa trên số chèn chứ không phải chỉ mục mảng dựa trên số 0. Trước khi đánh dấu một nhóm là đã được sử dụng, mã sẽ kiểm tra xem nó đã được sử dụng chưa. Thứ tự này quan trọng vì việc thay đổi giá trị trước tiên sẽ che giấu xung đột. 

Số nguyên Python xử lý trực tiếp các giá trị đầu vào lớn, do đó không cần xử lý tràn đặc biệt. Hoạt động modulo ngay lập tức giảm mọi giá trị thành chỉ mục nhóm hợp lệ trong khoảng`0`Và`p - 1`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
10 5
0
21
53
41
53
```| Bước | Giá trị | Xô | Xô đã được sử dụng trước đây? | Hành động | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | Không | Đánh dấu nhóm 0 | Tiếp tục | 
| 2 | 21 | 1 | Không | Đánh dấu thùng 1 | Tiếp tục | 
| 3 | 53 | 3 | Không | Đánh dấu thùng 3 | Tiếp tục | 
| 4 | 41 | 1 | Có | Đã tìm thấy xung đột | 4 | 

Dấu vết cho thấy lý do tại sao thuật toán lưu trữ các nhóm thay vì giá trị. Các giá trị`21`Và`41`khác nhau, nhưng chúng va chạm nhau vì cả hai đều ánh xạ tới nhóm`1`. 

### Mẫu 2 

đầu vào:```
5 5
0
1
2
3
4
```| Bước | Giá trị | Xô | Xô đã được sử dụng trước đây? | Hành động | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | Không | Đánh dấu nhóm 0 | Tiếp tục | 
| 2 | 1 | 1 | Không | Đánh dấu thùng 1 | Tiếp tục | 
| 3 | 2 | 2 | Không | Đánh dấu thùng 2 | Tiếp tục | 
| 4 | 3 | 3 | Không | Đánh dấu thùng 3 | Tiếp tục | 
| 5 | 4 | 4 | Không | Đánh dấu thùng 4 | -1 | 

Điều này chứng tỏ rằng việc đổ đầy mỗi thùng không có nghĩa là xảy ra va chạm. Câu trả lời chỉ là liệu phần chèn có cố gắng sử dụng lại nhóm hiện có hay không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi số được xử lý một lần với thao tác tra cứu và modulo theo thời gian không đổi. | 
| Không gian | O(p) | Thuật toán lưu trữ một giá trị boolean cho mỗi nhóm. | 

Kích thước đầu vào tối đa là rất nhỏ nên phương pháp này dễ dàng phù hợp với giới hạn về thời gian và bộ nhớ. Nó cũng chia tỷ lệ tuyến tính nếu số lần chèn tăng lên. 

## Trường hợp thử nghiệm```python
import sys
import io

def solution(inp: str) -> str:
    data = inp.strip().split()
    if not data:
        return ""

    p = int(data[0])
    n = int(data[1])
    used = [False] * p

    index = 2
    for i in range(1, n + 1):
        x = int(data[index])
        index += 1
        bucket = x % p
        if used[bucket]:
            return str(i)
        used[bucket] = True

    return "-1"

# provided samples
assert solution("""10 5
0
21
53
41
53
""") == "4", "sample 1"

assert solution("""5 5
0
1
2
3
4
""") == "-1", "sample 2"

# custom cases
assert solution("""2 2
0
2
""") == "2", "same bucket collision"

assert solution("""3 3
1000000000
1000000001
1000000002
""") == "-1", "large values with unique buckets"

assert solution("""10 4
7
17
27
37
""") == "2", "repeated bucket pattern"

assert solution("""300 1
299999999
""") == "-1", "single insertion boundary case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hai giá trị có cùng số dư | 2 | Phát hiện va chạm ngay lập tức | 
| Số lớn có số dư khác nhau | -1 | Xử lý modulo đúng | 
| Một số giá trị chia sẻ một nhóm | 2 | Dừng lại ở xung đột đầu tiên | 
| Một lần chèn | -1 | Xử lý đầu vào tối thiểu | 

## Vỏ cạnh 

Đối với trường hợp các giá trị khác nhau xung đột, chẳng hạn như:```
5 3
1
6
11
```các quy trình thuật toán`1`và đánh dấu xô`1`. Khi nó đạt tới`6`, nó tính toán`6 mod 5 = 1`, nhìn thấy cái xô đó`1`đã được sử dụng và ngay lập tức xuất ra`2`. Nó không bao giờ cần kiểm tra các giá trị còn lại vì xung đột đầu tiên đã được tìm thấy. 

Để điền bảng hoàn toàn hợp lệ:```
4 4
0
1
2
3
```mỗi giá trị ánh xạ tới một nhóm khác nhau. Thuật toán đánh dấu các nhóm`0`,`1`,`2`, Và`3`theo thứ tự. Không gặp phải nhóm bị chiếm dụng nào, vì vậy sau vòng lặp, nó sẽ trả về`-1`. 

Đối với các giá trị lặp lại:```
10 2
123456789
123456789
```nhóm dấu chèn đầu tiên`9`. Lần chèn thứ hai tính toán chỉ số nhóm tương tự và thấy nó bị chiếm dụng. Báo cáo thuật toán`2`, phù hợp với vị trí xung đột đầu tiên được yêu cầu.
