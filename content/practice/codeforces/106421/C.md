---
title: "CF 106421C - Cuộc gặp gỡ của nhà vô địch (Dễ)"
description: "Chúng tôi có hai đội đua được đặt hàng. Mỗi đội có một danh sách cấp độ uy tín và chúng ta cần xây dựng một đội duy nhất bằng cách lấy các tay đua từ cả hai danh sách trong khi vẫn giữ thứ tự ban đầu trong mỗi đội."
date: "2026-06-25T09:41:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106421
codeforces_index: "C"
codeforces_contest_name: "UTPC Contest 3-11-2026 Div. 2 (Advanced)"
rating: 0
weight: 106421
solve_time_s: 35
verified: true
draft: false
---

[CF 106421C - Cuộc gặp gỡ của nhà vô địch (Dễ dàng)](https://codeforces.com/problemset/problem/106421/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 35s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có hai đội đua được đặt hàng. Mỗi đội có một danh sách cấp độ uy tín và chúng ta cần xây dựng một đội duy nhất bằng cách lấy các tay đua từ cả hai danh sách trong khi vẫn giữ thứ tự ban đầu trong mỗi đội. Đầu ra không phải là chuỗi uy tín được hợp nhất mà là các lựa chọn tạo ra nó: mỗi nhân vật cho biết tay đua tiếp theo đến từ Đội A hay Đội B. 

Mục đích là làm cho chuỗi uy tín thu được nhỏ nhất về mặt từ điển. Vì số uy tín nhỏ hơn có nghĩa là tay đua có uy tín hơn nên một chuỗi được coi là tốt hơn khi đặt số nhỏ hơn càng sớm càng tốt. 

Phiên bản dễ dàng có`k = n + m`, và mỗi tay đua đều có một mức uy tín riêng. Tính duy nhất này là hạn chế chính. Tổng số tay đua có thể lên tới`400000`, do đó, một thuật toán liên tục quét các tay đua còn lại hoặc thử tất cả các lần xen kẽ có thể thực hiện được sẽ thực hiện quá nhiều thao tác. Chúng ta cần một giải pháp tuyến tính trong đó mỗi tay đua chỉ được xử lý một số lần không đổi. 

Một sai lầm phổ biến là nghĩ về vị thế tương lai thay vì lựa chọn hiện tại. Ví dụ, hãy xem xét:```
n = 2, m = 2
A = [1, 4]
B = [2, 3]
```Đầu ra đúng là:```
AABB
```bởi vì trình tự hợp nhất trở thành`[1,2,3,4]`. Chiến lược chọn từ B vì thấy rằng B còn lại hai giá trị nhỏ sẽ thua, vì phần tử đầu tiên sẽ trở thành`2`thay vì`1`. 

Một trường hợp khó khăn khác là khi một đội hết tay đua:```
n = 2, m = 1
A = [2, 3]
B = [1]
```Đầu ra đúng là:```
BAA
```Sau khi lấy`1`từ Đội B, chỉ còn lại Đội A. Việc triển khai bất cẩn giả định rằng cả hai vị trí hiện tại luôn tồn tại có thể truy cập vào các chỉ mục không hợp lệ hoặc không thể nối thêm các tay đua còn lại. 

Trường hợp quan trọng cuối cùng đã được xử lý bằng các ràng buộc, nhưng nó giải thích tại sao phiên bản dễ dàng lại đơn giản hơn:```
n = 2, m = 2
A = [1, 2]
B = [1, 3]
```Nếu cho phép mức độ uy tín trùng lặp, việc chỉ so sánh các tay đua hiện tại sẽ không phải lúc nào cũng quyết định chính xác vì các giá trị bằng nhau đòi hỏi phải xem xét sâu hơn các hậu tố còn lại. Phiên bản dễ dàng loại bỏ vấn đề này vì mỗi cấp độ uy tín chỉ xuất hiện một lần. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là tạo ra mọi sự đan xen hợp lệ có thể có của hai đội, tính toán trình tự uy tín thu được và giữ chuỗi uy tín nhỏ nhất về mặt từ điển. Điều này đúng vì nó trực tiếp kiểm tra mọi câu trả lời có thể. Tuy nhiên, số lượng xen kẽ là rất lớn. Chọn cái nào`n`vị trí thuộc về Đội A trong số`n+m`vị trí mang lại`C(n+m,n)`khả năng. Ngay cả đối với vài chục tay đua, điều này trở nên bất khả thi và với hàng trăm nghìn tay đua thì điều đó càng không khả thi. 

Cấu trúc của phiên bản dễ dàng mang lại cách tiếp cận đơn giản hơn nhiều. Tại bất kỳ thời điểm nào, tay đua tiếp theo trong đội đã hợp nhất phải là tay đua hiện tại của Đội A hoặc tay đua hiện tại của Đội B. Vì tất cả các cấp độ uy tín là khác nhau nên hai giá trị này không thể bằng nhau. Câu trả lời nhỏ hơn phải được chọn vì vị trí đầu tiên nơi hai câu trả lời có thể khác nhau sẽ xác định trình tự nào nhỏ hơn về mặt từ điển. 

Phương pháp vũ phu hoạt động vì nó khám phá mọi lựa chọn, nhưng không thành công vì không gian lựa chọn tăng theo cấp số nhân. Nhận xét rằng vị trí khác nhau đầu tiên hoàn toàn quyết định sự so sánh cho phép chúng ta đưa ra từng quyết định một cách tham lam và loại bỏ ngay lập tức mọi lựa chọn tồi tệ hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(C(n+m,n) * (n+m)) | O(n+m) | Quá chậm | 
| Tối ưu | O(n+m) | O(1) ngoài đầu ra | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Giữ hai con trỏ, một con trỏ trỏ đến tay đua chưa sử dụng tiếp theo trong Đội A và một con trỏ chỉ tay đua không sử dụng tiếp theo trong Đội B. 
2. Trong khi cả hai đội vẫn còn sẵn tay đua, hãy so sánh mức độ uy tín hiện tại của hai đội. Nối thêm`A`nếu tay đua của Đội A có giá trị nhỏ hơn, nếu không thì nối thêm`B`. Điều này đặt tay đua nhỏ nhất có thể vào vị trí chưa quyết định sớm nhất. 
3. Khi một đội không còn tay đua nào, hãy thêm các lựa chọn còn lại từ đội kia. Không còn phải đưa ra quyết định nào nữa vì mọi tay đua còn lại đều phải xuất hiện. 
4. In trình tự đã xây dựng của`A`Và`B`nhân vật. 

Tại sao nó hoạt động: Tại mọi vị trí trong câu trả lời, yếu tố tiếp theo duy nhất có thể là hai tay đua hiện tại. Nếu chúng ta chọn cái lớn hơn, thì chuỗi thay thế ngay lập tức có giá trị nhỏ hơn ở vị trí chính xác này, do đó lựa chọn của chúng ta không thể tối thiểu về mặt từ điển. Bởi vì mọi lựa chọn đều giữ nguyên thứ tự ban đầu của cả hai đội, nên việc liên tục đưa ra lựa chọn nhỏ nhất có sẵn sẽ tạo ra sự hợp nhất hợp lệ nhỏ nhất trên toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    i = 0
    j = 0
    ans = []

    while i < n and j < m:
        if a[i] < b[j]:
            ans.append('A')
            i += 1
        else:
            ans.append('B')
            j += 1

    while i < n:
        ans.append('A')
        i += 1

    while j < m:
        ans.append('B')
        j += 1

    print(''.join(ans))

if __name__ == "__main__":
    solve()
```Mã tuân theo quy trình tham lam một cách trực tiếp. Các biến`i`Và`j`luôn đề cập đến tay đua chưa được sử dụng tiếp theo trong đội tương ứng của họ. Vòng lặp đầu tiên xử lý trường hợp thú vị duy nhất, trong đó cả hai lựa chọn đều có sẵn. 

Vòng thứ hai và thứ ba được tách ra vì sau khi một bên cạn kiệt thì không còn gì để so sánh. Điều này cũng tránh được các lỗi ranh giới khi cố gắng truy cập`a[i]`hoặc`b[j]`sau khi đến cuối danh sách. 

Giá trị của`k`được đọc vì nó thuộc định dạng đầu vào, nhưng nó không cần thiết trong phiên bản dễ dàng. Sự thật là`k = n + m`đảm bảo rằng tất cả các giá trị uy tín là duy nhất, đó chính xác là tài sản được sử dụng bởi sự so sánh tham lam. 

## Ví dụ đã hoạt động 

Hãy xem xét:```
3 2 5
1 4 5
2 3
```Dấu vết là: 

| Bước | Con trỏ đội A | Con trỏ đội B | Hiện tại A | Hiện tại B | Lựa chọn | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 1 | 2 | A | A | 
| 2 | 1 | 0 | 4 | 2 | B | AB | 
| 3 | 1 | 1 | 4 | 3 | B | ABB | 
| 4 | 1 | 2 | 4 | không | A | ABBA | 
| 5 | 2 | 2 | 5 | không | A | ABBAA | 

Kết quả là`ABBAA`, tạo nên dãy uy tín sáp nhập`[1,2,3,4,5]`. Điều này chứng tỏ rằng một khi một bên trống thì phần còn lại của câu trả lời là bắt buộc. 

Một ví dụ khác:```
2 3 5
2 5
1 3 4
```Dấu vết là: 

| Bước | Con trỏ đội A | Con trỏ đội B | Hiện tại A | Hiện tại B | Lựa chọn | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 2 | 1 | B | B | 
| 2 | 0 | 1 | 2 | 3 | A | BA | 
| 3 | 1 | 1 | 5 | 3 | B | BÁ | 
| 4 | 1 | 2 | 5 | 4 | B | BABB | 
| 5 | 1 | 3 | 5 | không | A | BABBA | 

Đầu ra là`BABBA`. Thuật toán luôn chọn mức uy tín nhỏ nhất hiện có, tạo ra`[1,2,3,4,5]`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n+m) | Mỗi tay đua được xem xét một lần và đóng góp một ký tự cho câu trả lời. | 
| Không gian | O(n+m) | Chuỗi đầu ra lưu trữ chính xác một ký tự cho mỗi tay đua. | 

Kích thước đầu vào tối đa là hàng trăm nghìn tay đua, vì vậy việc quét tuyến tính là cần thiết. Giải pháp chỉ thực hiện các phép so sánh và nối thêm đơn giản, phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(data):
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(data)
    out = io.StringIO()
    sys.stdout = out

    input = sys.stdin.readline

    n, m, k = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    i = 0
    j = 0
    ans = []

    while i < n and j < m:
        if a[i] < b[j]:
            ans.append('A')
            i += 1
        else:
            ans.append('B')
            j += 1

    while i < n:
        ans.append('A')
        i += 1

    while j < m:
        ans.append('B')
        j += 1

    print(''.join(ans))

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out.getvalue()

assert solve("""3 2 5
1 4 5
2 3
""") == "ABBAA\n", "basic merge"

assert solve("""2 3 5
2 5
1 3 4
""") == "BABBA\n", "smallest starts from B"

assert solve("""1 1 2
2
1
""") == "BA\n", "minimum size"

assert solve("""5 1 6
1 2 3 4 6
5
""") == "AAAABA\n", "boundary with one racer in B"

assert solve("""4 4 8
1 3 5 7
2 4 6 8
""") == "ABABABAB\n", "alternating values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 2 5 / 1 4 5 / 2 3`|`ABBAA`| Sự hợp nhất tham lam bình thường | 
|`2 3 5 / 2 5 / 1 3 4`|`BABBA`| Đội thứ hai có thể cung cấp tay đua đầu tiên | 
|`1 1 2 / 2 / 1`|`BA`| Kích thước đầu vào tối thiểu và so sánh ngay lập tức | 
|`5 1 6 / 1 2 3 4 6 / 5`|`AAAABA`| Xử lý đúng khi một đội gần trống | 
|`4 4 8 / 1 3 5 7 / 2 4 6 8`|`ABABABAB`| Lựa chọn ranh giới lặp đi lặp lại | 

## Vỏ cạnh 

Đối với trường hợp cạnh đầu tiên:```
2 2 4
1 4
2 3
```Thuật toán so sánh`1`Và`2`, chọn`A`, sau đó so sánh`4`Và`2`, chọn`B`, sau đó so sánh`4`Và`3`, chọn`B`, và cuối cùng nối thêm phần còn lại`A`. Đầu ra là`ABBA`. Quyết định tham lam là đúng đắn vì đã chọn`B`đầu tiên sẽ làm cho chuỗi hợp nhất bắt đầu bằng`2`, điều này còn tệ hơn việc bắt đầu bằng`1`. 

Đối với trường hợp đội kiệt sức:```
2 1 3
2 3
1
```Sự so sánh đầu tiên chọn`B`bởi vì`1`nhỏ hơn`2`. Khi đó Đội B trống nên thuật toán sẽ thêm hai tay đua của Đội A còn lại vào. Đầu ra là`BAA`, và trình tự hợp nhất là`[1,2,3]`. 

Đối với trường hợp giá trị trùng lặp sẽ xuất hiện trong phiên bản khó hơn:```
2 2 3
1 2
1 3
```Việc so sánh chỉ các giá trị hiện tại sẽ không đủ vì cả hai lựa chọn đều bắt đầu bằng`1`. Phiên bản dễ dàng tránh được sự mơ hồ này vì tất cả các giá trị là duy nhất, chỉ cho phép so sánh hiện tại để xác định quyết định chính xác.
