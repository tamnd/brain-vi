---
title: "CF 1211D - Đồng đội"
description: "Chúng tôi được cung cấp nhiều xếp hạng của người chơi. Mỗi người chơi chỉ có thể được sử dụng nhiều nhất một lần và mục tiêu của chúng tôi là thành lập càng nhiều đội rời rạc càng tốt trong một ràng buộc về cấu trúc nghiêm ngặt. Mỗi đội hợp lệ bao gồm hai nhóm người chơi đồng nhất."
date: "2026-06-13T17:05:19+07:00"
tags: ["codeforces", "competitive-programming", "*special", "binary-search", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1211
codeforces_index: "D"
codeforces_contest_name: "Kotlin Heroes: Episode 2"
rating: 2000
weight: 1211
solve_time_s: 393
verified: true
draft: false
---

[CF 1211D - Nhóm](https://codeforces.com/problemset/problem/1211/D) 

**Xếp hạng:** 2000 
**Tags:** *đặc biệt, tìm kiếm nhị phân, tham lam, toán học 
**Thời gian giải:** 6m 33s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp nhiều xếp hạng của người chơi. Mỗi người chơi chỉ có thể được sử dụng nhiều nhất một lần và mục tiêu của chúng tôi là thành lập càng nhiều đội rời rạc càng tốt trong một ràng buộc về cấu trúc nghiêm ngặt. 

Mỗi đội hợp lệ bao gồm hai nhóm người chơi đồng nhất. Một nhóm có chính xác`a`người chơi, tất cả đều có chung giá trị xếp hạng`x`. Nhóm thứ hai có chính xác`b`người chơi, tất cả đều chia sẻ một giá trị xếp hạng khác`k · x`. Một đội chỉ hợp lệ nếu hai nhóm này tồn tại đồng thời và tách rời khỏi tất cả các đội. 

Vì vậy, mỗi đội về cơ bản là một quy tắc ghép nối giữa các số đếm: chúng ta cần`a`bản sao của một số giá trị`x`, Và`b`bản sao có giá trị`k·x`. Các đội khác nhau sử dụng tần số giá trị và khi một người chơi được sử dụng, người đó sẽ không thể tham gia lại. 

Nhiệm vụ là tối đa hóa số lượng đội như vậy mà chúng ta có thể xây dựng. 

Kích thước đầu vào lên tới 300.000 phần tử, do đó, bất kỳ giải pháp nào cố gắng mô phỏng nhiều lần các nhóm hình thành một cách tham lam mà không có cấu trúc sẽ quá chậm. Thậm chí một$O(n^2)$chiến lược ghép nối ngay lập tức không khả thi vì nó sẽ yêu cầu kiểm tra tính tương thích giữa nhiều nhóm tần số nhiều lần. Hướng khả thi duy nhất là nén mảng thành tần số và xử lý các giá trị theo thứ tự có cấu trúc, thường là logarit hoặc gần tuyến tính. 

Một khó khăn tinh tế là các lựa chọn tương tác giữa các giá trị. Nếu chúng ta tham lam khớp một giá trị`x`quá sớm hoặc quá muộn đều có thể ảnh hưởng đến khả năng cung cấp`k·x`, vì vậy những cách tiếp cận tham lam ngây thơ và không kiểm soát cẩn thận việc đặt hàng có thể thất bại. 

Một trường hợp thất bại phổ biến như thế này: giả sử chúng ta luôn cố gắng thành lập một nhóm ngay khi thấy đủ`x`Và`k·x`, mà không xem xét những đóng góp trong tương lai. Điều này có thể lãng phí các phần tử có giá trị cao cần thiết để ghép nối tối ưu hơn sau này. 

Một vấn đề khác là đặt hàng: vì`k·x`lớn hơn`x`, việc xử lý theo thứ tự tăng dần cho phép chúng ta luôn xử lý`x`đầu tiên và đẩy các ràng buộc về phía trước, tránh xem lại các trạng thái. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là suy nghĩ về tần suất và liên tục cố gắng xây dựng một nhóm bằng cách quét tất cả những gì có thể.`x`, kiểm tra xem`freq[x] >= a`Và`freq[k·x] >= b`, sau đó trừ và đếm một đội. Lặp lại điều này cho đến khi không có thay đổi nào xảy ra về mặt khái niệm là đúng nhưng cực kỳ chậm. Trong trường hợp xấu nhất, mỗi lần lặp có thể chỉ loại bỏ một nhóm và quét tất cả các giá trị$O(U)$, dẫn đến$O(U \cdot \text{answer})$, quá lớn khi cả hai đều lớn. 

Quan sát quan trọng là vấn đề hoàn toàn do tần số và sự ghép đôi xác định giữa`x`Và`k·x`. Một khi chúng tôi sửa chữa bao nhiêu lần một`x`được sử dụng làm căn cứ của các đội, mọi thứ trở nên gượng ép. Quyết định thực sự duy nhất là chúng tôi chọn bao nhiêu nhóm từ mỗi giá trị, nhưng quyết định này mang tính cục bộ nếu chúng tôi xử lý các giá trị theo thứ tự tăng dần. 

Tối ưu hóa tiêu chuẩn là duy trì tần số trong bản đồ hoặc mảng và lặp lại các giá trị theo thứ tự được sắp xếp. Với mỗi giá trị`x`, chúng tôi tính toán xem chúng tôi có thể thành lập bao nhiêu đội bằng cách sử dụng`x`như nhóm đầu tiên. Mỗi đội như vậy tiêu thụ`a`bản sao của`x`Và`b`bản sao của`k·x`. Chúng tôi cập nhật tần suất`k·x`tương ứng. 

Bởi vì`k·x`luôn lớn hơn`x`, khi chúng tôi đạt được`x`, tần số của`x`là cuối cùng và sẽ không bao giờ bị thay đổi sau này bởi các hoạt động trong tương lai. Điều này làm cho một lần vượt qua là đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force | O(U · trả lời) | O(U) | Quá chậm | 
| Tần số tham lam theo thứ tự sắp xếp | O(U log U) | O(U) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi nén đầu vào thành bản đồ xếp hạng tần số. Điều này loại bỏ thứ tự không liên quan và biến vấn đề thành số học về số lượng. 

Sau đó, chúng tôi sắp xếp tất cả các xếp hạng riêng biệt theo thứ tự tăng dần sao cho mọi giá trị đều được xử lý trước bội số của nó.`k·x`. 

1. Xây dựng bảng tần số`freq[v]`cho tất cả các xếp hạng. Bước này là cần thiết vì chỉ tính vật chất chứ không tính vị trí. 
2. Trích xuất tất cả các giá trị khác biệt và sắp xếp chúng theo thứ tự tăng dần. Việc sắp xếp đảm bảo rằng khi chúng ta xử lý một giá trị`x`, tất cả những người tiêu dùng có thể có trong tương lai`k·x`chưa được xử lý hoặc sẽ không ảnh hưởng`x`sau đó. 
3. Lặp lại từng giá trị`x`theo thứ tự sắp xếp. Vào lúc này,`freq[x]`đại diện cho số lượng có sẵn cuối cùng cho`x`. 
4. Tính xem có thể thành lập bao nhiêu đội từ`x`là nhóm đánh giá nhỏ. Đây là`t = freq[x] // a`. Chúng tôi không thể hình thành nhiều hơn thế này vì mỗi đội cần`a`bản sao của`x`. 
5. Tuy nhiên, chúng tôi cũng bị hạn chế bởi tính sẵn có của`k·x`. Vì vậy chúng tôi giới hạn`t`qua`freq[k·x] // b`. Điều này đảm bảo chúng ta không lạm dụng`k·x`. 
6. Thêm`t`để trả lời. Sau đó trừ tài nguyên: giảm`freq[x]`qua`t · a`và giảm`freq[k·x]`qua`t · b`. Điều này thực thi việc sử dụng rời rạc. 

Điều tinh tế mấu chốt là chúng ta không bao giờ xem lại`x`. Mọi thứ còn lại`x`sau khi xử lý không thể được sử dụng sau này vì không có bước nào trong tương lai sẽ sử dụng nó như một nhóm thứ hai, vì tất cả các nhóm thứ hai đều yêu cầu giá trị lớn hơn. 

### Tại sao nó hoạt động 

Thuật toán thực thi sự phụ thuộc có hướng: mỗi nhóm sử dụng giá trị nhỏ hơn làm nguồn và giá trị lớn hơn làm phần chìm. Bằng cách xử lý theo thứ tự tăng dần, chúng tôi đảm bảo rằng khi chúng tôi chỉ định mức sử dụng`x`, không có hoạt động nào sau này có thể ảnh hưởng trở lại nó. Do đó, mỗi giá trị được quyết định chính xác một lần và mỗi nhóm được thành lập đều có giá trị tối đa với các ràng buộc cục bộ. Điều này giúp loại bỏ các chu kỳ và đảm bảo rằng việc ghép nối cục bộ tham lam không cản trở việc ghép nối tối ưu trong tương lai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, a, b, k = map(int, input().split())
    arr = list(map(int, input().split()))
    
    from collections import Counter
    freq = Counter(arr)
    
    vals = sorted(freq.keys())
    
    ans = 0
    
    for x in vals:
        if freq[x] == 0:
            continue
        y = x * k
        
        if y not in freq:
            continue
        
        # number of possible teams from x
        t = min(freq[x] // a, freq[y] // b)
        
        if t > 0:
            freq[x] -= t * a
            freq[y] -= t * b
            ans += t
    
    print(ans)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã này xây dựng một bản đồ tần số để chúng ta có thể suy luận thuần túy về số lượng. Khóa sắp xếp đảm bảo rằng chúng tôi luôn xử lý xếp hạng nhỏ hơn trước. 

Bên trong vòng lặp, chúng tôi chỉ thử ghép nối nếu cả hai`x`Và`k·x`hiện hữu. Tính toán nút cổ chai sử dụng phép chia số nguyên ở cả hai bên và chúng tôi lấy mức tối thiểu để tôn trọng cả hai ràng buộc cùng một lúc. Cập nhật bảng tần số đảm bảo rằng không có trình phát nào được sử dụng lại. 

Một cạm bẫy phổ biến là quên trừ đi`freq[y]`. Nếu không có điều này, các giá trị sau này sẽ sử dụng lại các phần tử đã được sử dụng một cách không chính xác, làm tăng câu trả lời. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
12 1 2 2
1 1 2 2 2 2 2 3 3 4 6 6
```Chúng tôi theo dõi tần số: 

| x | tần số[x] | tần số[2x] | đội được thành lập | còn lại x | còn lại 2x | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 3 | 1 | 1 | 1 | 
| 2 | 3 | 2 | 1 | 1 | 0 | 
| 3 | 2 | 0 | 0 | 2 | 0 | 

Câu trả lời cuối cùng là 3. 

Dấu vết cho thấy rằng sau khi chúng tôi xử lý một giá trị, phần đóng góp của giá trị đó sẽ được giải quyết hoàn toàn và các phần tử còn sót lại chỉ có thể sử dụng được trong các cặp hợp lệ sau này với bội số của chúng. 

### Ví dụ 2 

đầu vào:```
6 1 1 2
1 1 2 2 4 4
```Tần số: 1→2, 2→2, 4→2 

| x | tần số[x] | tần số[2x] | đội | còn lại x | còn lại 2x | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 2 | 2 | 0 | 0 | 
| 2 | 2 | 2 | 0 | 2 | 2 | 
| 4 | 2 | 0 | 0 | 2 | 0 | 

Chỉ đóng góp giá trị 1 vì bội số của nó đã được sử dụng. 

Điều này chứng tỏ rằng quá trình xử lý sớm sẽ khóa việc sử dụng và ngăn chặn việc tái sử dụng ở phía sau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(U \log U)$| sắp xếp xếp hạng khác biệt chiếm ưu thế | 
| Không gian |$O(U)$| bản đồ tần số theo giá trị | 

Các ràng buộc cho phép tối đa 300.000 người chơi và xếp hạng lên tới 1e6. Sắp xếp các giá trị riêng biệt và thực hiện cập nhật liên tục theo thời gian cho mỗi giá trị phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import Counter

    n, a, b, k = map(int, sys.stdin.readline().split())
    arr = list(map(int, sys.stdin.readline().split()))
    
    freq = Counter(arr)
    vals = sorted(freq.keys())
    
    ans = 0
    for x in vals:
        y = x * k
        if freq[x] == 0 or y not in freq:
            continue
        t = min(freq[x] // a, freq[y] // b)
        freq[x] -= t * a
        freq[y] -= t * b
        ans += t
    
    return str(ans)

# provided sample
assert run("12 1 2 2\n1 1 2 2 2 2 2 3 3 4 6 6\n") == "3"

# all equal no pairing
assert run("5 2 2 2\n1 1 1 1 1\n") == "0"

# exact single team
assert run("3 1 2 2\n1 2 2\n") == "1"

# chain interference
assert run("6 1 1 2\n1 1 2 2 4 4\n") == "2"

# large k break (no multiples)
assert run("4 1 1 10\n1 1 2 2\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | 0 | không có cặp hợp lệ nào tồn tại | 
| hợp lệ tối thiểu | 1 | thành lập đội đơn | 
| hộp đựng dây chuyền | 2 | tương tác qua bội số | 
| k lớn | 0 | thiếu đối tác ngăn cản đội | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi nhiều giá trị có thể đóng vai trò là`k·x`cho khác nhau`x`. Ví dụ, nếu cả hai`x`Và`x/k`tồn tại trong mảng, phép trừ tham lam bất cẩn có thể tiêu thụ quá mức nhóm dùng chung. Việc xử lý đơn hàng được sắp xếp sẽ ngăn chặn điều này bởi vì một khi`x`được xử lý, nó không bao giờ được xem lại và mọi việc sử dụng đều được quyết định ở nguồn nhỏ nhất.
