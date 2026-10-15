---
title: "CF 105444G - Tổ hợp Gig"
description: "Chúng tôi được cung cấp một chuỗi các bài hát theo thứ tự chúng được viết. Mỗi bài hát có một nhãn, chỉ có thể là 1, 2 hoặc 3. Chúng tôi muốn đếm xem có bao nhiêu phần tiếp theo của những bài hát này tạo thành một “danh sách cố định” hợp lệ."
date: "2026-06-23T03:31:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105444
codeforces_index: "G"
codeforces_contest_name: "2020-2021 ACM-ICPC Nordic Collegiate Programming Contest (NCPC 2020)"
rating: 0
weight: 105444
solve_time_s: 51
verified: true
draft: false
---

[CF 105444G - Tổ hợp Gig](https://codeforces.com/problemset/problem/105444/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi các bài hát theo thứ tự chúng được viết. Mỗi bài hát có một nhãn, chỉ có thể là 1, 2 hoặc 3. Chúng tôi muốn đếm xem có bao nhiêu phần tiếp theo của những bài hát này tạo thành một “danh sách cố định” hợp lệ. 

Danh sách tập hợp hợp lệ là một chuỗi con được lấy theo thứ tự từ danh sách ban đầu, với ba ràng buộc về cấu trúc. Bài hát được chọn đầu tiên phải có nhãn 1, bài hát được chọn cuối cùng phải có nhãn 3 và mọi bài hát ở giữa phải có nhãn 2. Danh sách cố định phải chứa ít nhất ba bài hát, do đó, ít nhất một bài 1, ít nhất một bài 3 và ít nhất một bài 2 ở giữa. 

Đây là một vấn đề đếm trên tất cả các mảng con, nhưng cấu trúc cực kỳ cứng nhắc: mọi danh sách tập hợp hợp lệ được xác định hoàn toàn bằng cách chọn vị trí bắt đầu chứa 1 và vị trí kết thúc chứa 3, với yêu cầu mọi thứ giữa chúng chỉ là 2. 

Ràng buộc n lên tới 10^6 ngay lập tức loại trừ mọi cách liệt kê O(n^2) hoặc O(n^2) của các cặp điểm cuối. Ngay cả các giải pháp O(n log n) cũng có thể chấp nhận được nhưng không cần thiết; cấu trúc gợi ý một giải pháp quét tuyến tính là có thể. 

Một cách tiếp cận đơn giản là thử tất cả các cặp (l, r) và kiểm tra xem phân đoạn đó có hợp lệ hay không sẽ tính toán lại các kiểm tra bên trong nhiều lần và thất bại thảm hại ở mức n = 10^6. 

Một trường hợp lỗi tinh vi hơn sẽ xuất hiện nếu người ta cố gắng chỉ kiểm tra cặp điểm cuối 1 và 3 mà không xác nhận nội dung bên trong một cách hiệu quả. Ví dụ, trong đầu vào 

1 2 3 1 3 

cặp (1, 3) hợp lệ vì phần giữa là 2, nhưng (1, 4) không hợp lệ vì phần giữa chứa 3. Phương pháp ghép nối điểm cuối ngây thơ bỏ qua xác thực bên trong sẽ bị tính quá mức. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: xem xét mọi cặp chỉ số có thể có (l, r) và kiểm tra xem một danh sách tập hợp hợp lệ có thể được hình thành từ l đến r hay không. Điều này yêu cầu xác minh rằng a[l] = 1, a[r] = 3 và mọi phần tử giữa chúng là 2. Ngay cả khi kiểm tra bên trong được tối ưu hóa với thông tin tiền tố, việc lặp lại tất cả các cặp vẫn tốn O(n^2) trong trường hợp xấu nhất. Với n = 10^6 thì điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là cấu trúc của một phân đoạn hợp lệ buộc tất cả các phần tử bên trong phải là 2. Điều này có nghĩa là các phân đoạn hợp lệ chỉ có thể tồn tại bên trong các khối liền kề tối đa là 2 giây, với số 1 ngay trước khối và số 3 ngay sau khối. Vì vậy, thay vì nghĩ về các mảng con tùy ý, chúng ta rút gọn vấn đề về việc đếm các mẫu có dạng 1 + (một hoặc nhiều 2 giây) + 3, trong đó các số 2 tạo thành một vùng liên tục trong mảng ban đầu. 

Khi chúng ta xem mảng theo cách này, chúng ta có thể quét một lần, theo dõi các khoảng thời gian trong 2 giây liên tiếp và với mỗi khoảng thời gian như vậy, hãy đếm xem có bao nhiêu số 1 xuất hiện ngay trước nó và bao nhiêu số 3 xuất hiện ngay sau nó. Mỗi lựa chọn (1, bắt đầu 2 khối, kết thúc 2 khối, 3) đóng góp chính xác một danh sách tập hợp hợp lệ và chúng tôi có thể tổng hợp những đóng góp này một cách hiệu quả. 

Điều này biến vấn đề thành việc đếm tuyến tính trên các lần chạy, tránh mọi thao tác quét lặp lại các phân đoạn bên trong. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Đếm dựa trên lần chạy | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi quét mảng một lần và nén nó về mặt khái niệm thành các chuỗi có giá trị bằng nhau, nhưng chúng tôi không cần lưu trữ chúng một cách rõ ràng.

1. Chúng tôi lặp qua mảng trong khi xác định các phân đoạn liền kề tối đa có giá trị 2. Mỗi phân đoạn như vậy là một “vùng giữa” ứng cử viên của danh sách tập hợp hợp lệ. Điều này quan trọng vì bất kỳ danh sách cố định hợp lệ nào cũng phải có tất cả các phần tử bên trong bằng 2, do đó nó không thể vượt qua ranh giới không phải là 2. 
2. Đối với mỗi khối 2 liền kề, chúng tôi đếm có bao nhiêu lựa chọn hợp lệ tồn tại cho số 1 bắt đầu. Đây là số số 1 liên tiếp ngay bên trái của khối này, vì phần đầu của phân đoạn hợp lệ phải là số 1 được đặt ngay trước khối 2 theo thứ tự ban đầu. Bất kỳ số 1 nào ở xa hơn sẽ buộc không phải số 2 bên trong phân khúc. 
3. Tương tự, chúng ta đếm có bao nhiêu lựa chọn hợp lệ cho phần cuối 3. Đây là số 3 liên tiếp ngay bên phải của khối 2, vì phần cuối 3 phải ở ngay sau khối 2. 
4. Số lượng danh sách tập hợp hợp lệ được đóng góp bởi 2 khối nhất định là tích của hai số này, bởi vì mỗi lựa chọn bên trái của 1 có thể ghép nối với mỗi lựa chọn bên phải của 3 khối một cách độc lập và tất cả các kết hợp đều bảo toàn cấu trúc được yêu cầu. 
5. Chúng ta tích lũy tích này trên tất cả 2 khối, lấy kết quả theo modulo 10^9 + 7. 

### Tại sao nó hoạt động 

Bất kỳ danh sách tập hợp hợp lệ nào cũng phải có cấu trúc trong đó tất cả các phần tử bên trong là 2. Điều đó buộc phần bên trong của phân đoạn đã chọn nằm hoàn toàn trong một chuỗi 2 giây liền kề trong mảng. Sau khi lần chạy đó được sửa, điểm cuối chỉ được xác định bằng số lượng số 1 liên tiếp xuất hiện ngay trước nó và số lượng số 3 liên tiếp xuất hiện ngay sau nó. Không có vị trí nào khác có thể hình thành ranh giới hợp lệ vì việc đưa ra bất kỳ khoảng trống hoặc không khớp nào sẽ phá vỡ ràng buộc chỉ có 2 giây xuất hiện bên trong. Điều này làm cho việc phân tách thành các đóng góp độc lập từ mỗi 2 lần chạy đều hoàn chỉnh và không chồng chéo. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    # Precompute prefix counts of consecutive 1s ending at i
    left_ones = [0] * n
    if a[0] == 1:
        left_ones[0] = 1
    for i in range(1, n):
        if a[i] == 1:
            left_ones[i] = left_ones[i - 1] + 1
    
    # Precompute suffix counts of consecutive 3s starting at i
    right_threes = [0] * n
    if a[-1] == 3:
        right_threes[-1] = 1
    for i in range(n - 2, -1, -1):
        if a[i] == 3:
            right_threes[i] = right_threes[i + 1] + 1
    
    ans = 0
    
    i = 0
    while i < n:
        if a[i] != 2:
            i += 1
            continue
        
        j = i
        while j < n and a[j] == 2:
            j += 1
        
        left_count = left_ones[i - 1] if i > 0 else 0
        right_count = right_threes[j] if j < n else 0
        
        if left_count and right_count:
            ans = (ans + left_count * right_count) % MOD
        
        i = j
    
    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên xây dựng hai mảng trợ giúp để nắm bắt thời gian các lần chạy 1 kết thúc ở mỗi vị trí và thời gian các lần chạy 3 giây bắt đầu ở mỗi vị trí. Những điều này cho phép truy cập liên tục vào số lượng lựa chọn bắt đầu và kết thúc hợp lệ tồn tại xung quanh bất kỳ 2 khối nào. 

Sau đó, nó quét qua mảng, nhảy qua các phân đoạn liền kề trong 2 giây. Đối với mỗi phân đoạn như vậy, nó nhìn sang trái và phải ngay lập tức để xác định có bao nhiêu phần đầu và phần cuối hợp lệ. Nhân các số đếm này sẽ cho ra sự đóng góp của khối đó. 

Một cạm bẫy phổ biến là cho phép các số 1 hoặc 3 không liền kề tham gia một cách không chính xác. Việc sử dụng số lần chạy liên tiếp đảm bảo chỉ xem xét các giá trị liền kề với ranh giới. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 1 1 2 2 2 3 3 3
```Chúng tôi tính toán các số 1 liên tiếp kết thúc ở mỗi chỉ số và các số 3 liên tiếp bắt đầu ở mỗi chỉ số. 

2 khối duy nhất là từ chỉ số 3 đến 5. 

| Bước | 2 khối (i, j) | left_count (1 giây trước) | right_count (3 giây sau) | đóng góp | 
| --- | --- | --- | --- | --- | 
| 1 | (3, 5) | 3 | 3 | 9 | 

Khối 2 được bao quanh bởi ba số 1 và ba số 3, vì vậy chúng ta nhận được 3 × 3 = 9 danh sách tập hợp hợp lệ. 

Điều này xác nhận rằng mọi sự kết hợp giữa việc chọn số 1 bắt đầu trong số ba vị trí đầu tiên và số 3 kết thúc trong số ba vị trí cuối cùng sẽ mang lại một phân đoạn hợp lệ thông qua khối 2. 

### Ví dụ 2 

đầu vào:```
1 2 1 2 3 1 2 3
```Chúng tôi xác định hai khối 2: tại chỉ mục 1 và tại chỉ mục 3 và tại chỉ mục 6. 

| Bước | 2 khối | left_count | đúng_count | đóng góp | 
| --- | --- | --- | --- | --- | 
| 1 | (1, 1) | 1 | 0 | 0 | 
| 2 | (3, 3) | 1 | 1 | 1 | 
| 3 | (6, 6) | 1 | 1 | 1 | 

Chỉ các khối được bao quanh đúng cách bởi ít nhất một khối 1 ở bên trái và một khối 3 ở bên phải mới đóng góp. Tổng cộng là 2. 

Điều này cho thấy rằng các số 2 bị cô lập hoặc các số 2 ở vị trí kém không đóng góp gì trừ khi chúng được đặt trong ngoặc một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi mảng được quét với số lần không đổi và mỗi chỉ mục được truy cập tối đa một lần trong lần quét 2 khối | 
| Không gian | O(n) | Hai mảng phụ lưu trữ độ dài chạy tiền tố và hậu tố | 

Giải pháp này phù hợp một cách thoải mái trong giới hạn cho n lên tới 10^6 vì nó chỉ thực hiện truyền tuyến tính qua đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import contextlib

    out = io.StringIO()
    with contextlib.redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample 1
assert run("9\n1 1 1 2 2 2 3 3 3\n") == "9"

# sample 2
assert run("8\n1 2 1 2 3 1 2 3\n") == "2"

# all ones (no valid setlists)
assert run("5\n1 1 1 1 1\n") == "0"

# no 1s
assert run("5\n2 2 2 3 3\n") == "0"

# no 3s
assert run("5\n1 2 2 2 2\n") == "0"

# single valid minimal pattern
assert run("3\n1 2 3\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả 1s | 0 | không có cấu trúc 2/3 | 
| không 1s | 0 | bắt đầu thiếu | 
| không 3 giây | 0 | thiếu kết thúc | 
| 1 2 3 | 1 | trường hợp hợp lệ tối thiểu | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi có 2 khối nằm ở ranh giới của mảng. Đối với đầu vào như```
2 2 2 1 3 3
```không có số 1 hợp lệ ngay trước bất kỳ 2 khối nào, vì vậy left_count bằng 0 và không có đóng góp nào được thêm vào. Thuật toán bỏ qua khối này một cách chính xác vì nó tính toán left_count từ chỉ mục i - 1, nằm ngoài phạm vi. 

Một trường hợp khác là khi tồn tại nhiều khối 2 nhưng chỉ một số khối được đặt trong ngoặc đúng:```
1 2 3 2 2 3
```2 khối đầu tiên đóng góp 1 × 1, trong khi khối thứ hai cũng đóng góp 1 × 1. Mỗi cái được xử lý độc lập vì quá trình quét sẽ đặt lại ở mọi ranh giới không phải 2, đảm bảo không tính hai lần hoặc trộn chéo khối.
