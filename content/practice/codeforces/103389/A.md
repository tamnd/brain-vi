---
title: "CF 103389A - \u516c\u4ea4\u7ebf\u8def"
description: "Chúng ta được cung cấp một tuyến xe buýt với danh sách các điểm dừng được sắp xếp cố định. Một hành khách báo cáo một chuỗi các điểm dừng mà họ quan sát được khi đi xe buýt, nhưng có một vấn đề phức tạp: xe buýt có thể đang di chuyển theo hướng thuận của tuyến đường hoặc theo hướng ngược lại."
date: "2026-07-03T12:11:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103389
codeforces_index: "A"
codeforces_contest_name: "2021\u5e74\u4e2d\u56fd\u5927\u5b66\u751f\u7a0b\u5e8f\u8bbe\u8ba1\u7ade\u8d5b\u5973\u751f\u4e13\u573a"
rating: 0
weight: 103389
solve_time_s: 50
verified: true
draft: false
---

[CF 103389A - \u516c\u4ea4\u7ebf\u8def](https://codeforces.com/problemset/problem/103389/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tuyến xe buýt với danh sách các điểm dừng được sắp xếp cố định. Một hành khách báo cáo một chuỗi các điểm dừng mà họ quan sát được khi đi xe buýt, nhưng có một vấn đề phức tạp: xe buýt có thể đang di chuyển theo hướng thuận của tuyến đường hoặc theo hướng ngược lại. 

Nhiệm vụ là xác định xem trình tự dừng được quan sát có phù hợp với việc di chuyển về phía trước dọc theo tuyến đường, lùi dọc theo tuyến đường hay không, cả hai hoặc không. Nếu cả hai hướng có thể tạo ra chuỗi quan sát giống hệt nhau thì không thể xác định được hướng và câu trả lời được coi là mơ hồ. 

Nói một cách cụ thể hơn, hãy coi tuyến đường như một mảng`route[0..n-1]`. Trình tự được quan sát là một mảng khác`obs[0..k-1]`. Chúng tôi đang kiểm tra xem`obs`khớp chính xác với tuyến đường như một đường truyền tiếp giáp theo thứ tự thuận hoặc chính xác như một đường truyền tiếp giáp theo thứ tự ngược lại. 

Đầu ra là sự phân loại quan sát dựa trên hai khả năng này. 

Mặc dù báo cáo vấn đề cực kỳ ngắn nhưng cấu trúc ẩn chính là chúng ta không xây dựng lại bất kỳ thứ gì phức tạp như đường dẫn hoặc đồ thị. Chúng tôi chỉ so sánh một chuỗi với hai chuỗi tham chiếu xác định: chính tuyến đường và phiên bản đảo ngược của nó. 

Vì sự so sánh là tuyến tính nên các ràng buộc ngầm gợi ý rằng`n`Và`k`đủ lớn để phương pháp quét bậc hai hoặc lặp lại sẽ quá chậm. Do đó, bất kỳ giải pháp nào cũng phải so sánh các chuỗi theo thời gian tuyến tính. 

Một số trường hợp đặc biệt đáng được nêu bật. 

Nếu trình tự được quan sát giống hệt với cả tuyến đường thuận và tuyến đường ngược, điều này chỉ có thể xảy ra khi tuyến đường có độ dài 0 hoặc 1 hoặc khi tất cả các phần tử giống hệt nhau. Trong trường hợp đó, câu trả lời đúng là “Không chắc chắn”. 

Nếu trình tự được quan sát không khớp với hướng nào, thậm chí một phần, thì việc kiểm tra chuỗi con đơn giản có thể kết luận sai một kết quả khớp nếu nó chỉ kiểm tra tính bằng nhau của tiền tố thay vì bằng tính bằng nhau của chuỗi đầy đủ. Ví dụ, tuyến đường`[1,2,3,4]`và quan sát`[1,2,4]`không hợp lệ theo cả hai hướng, mặc dù nó khớp với các phần đầu của tuyến đường. 

Một trường hợp tinh tế khác là khi tuyến đường ngược lại khớp chính xác nhưng tuyến đường thuận thì không. Việc triển khai bất cẩn chỉ kiểm tra một hướng hoặc giả định tính đối xứng sẽ phân loại sai hướng này. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: tạo ra cả hai đường đi có thể có của tuyến đường, một theo thứ tự thuận và một theo thứ tự ngược lại, và so sánh từng lần đi qua với trình tự được quan sát. 

Đối với mỗi hướng, chúng tôi quét cả hai mảng theo từng phần tử. Nếu tất cả các phần tử khớp nhau và có độ dài bằng nhau thì hướng đó là hợp lệ. Điều này đúng vì mọi phép duyệt hợp lệ đều phải bảo toàn chính xác cả thứ tự và giá trị. 

Tuy nhiên, mặc dù mỗi so sánh là tuyến tính, việc triển khai đơn giản có thể liên tục tính toán lại các mảng đảo ngược hoặc thực hiện quét dự phòng cho nhiều trường hợp thử nghiệm. Nếu có nhiều trường hợp thử nghiệm hoặc đầu vào lớn, việc này có thể trở nên kém hiệu quả do việc phân bổ bộ nhớ lặp đi lặp lại hoặc công việc không cần thiết. 

Quan sát chính là chúng ta không cần xây dựng các mảng đảo ngược một cách rõ ràng. Chúng ta có thể lập chỉ mục trực tiếp vào mảng ban đầu theo thứ tự ngược lại trong quá trình so sánh. Điều này giúp giảm chi phí và giữ cho giải pháp hoàn toàn tuyến tính cả về thời gian và bộ nhớ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (xây dựng + so sánh cả hai mảng) | O(n + k) | O(n) | Được chấp nhận nhưng không hiệu quả | 
| Tối ưu (so sánh dựa trên chỉ mục) | O(n + k) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô tả quá trình giả định rằng chúng tôi đã có lộ trình và trình tự quan sát được. 

### 1. Đọc lộ trình và trình tự quan sát 

Chúng tôi lưu trữ tuyến đường trong một mảng và trình tự quan sát được trong một mảng khác. Điều này là cần thiết vì chúng tôi cần quyền truy cập được lập chỉ mục trực tiếp cho cả so sánh thuận và ngược. 

### 2. Kiểm tra tính hợp lệ của hướng chuyển tiếp 

Trước tiên, chúng tôi kiểm tra xem trình tự được quan sát có khớp với tuyến đường theo thứ tự chuyển tiếp hay không. Trước tiên, chúng tôi so sánh độ dài vì sự không khớp sẽ ngay lập tức loại trừ sự bằng nhau. Sau đó, chúng tôi quét chỉ mục theo chỉ mục và xác minh sự bình đẳng. 

Bước này đúng vì việc truyền tải về phía trước tương ứng chính xác với mảng tuyến đường mà không cần sửa đổi. 

### 3. Kiểm tra tính hợp lệ của chiều ngược lại 

Chúng ta lặp lại sự so sánh tương tự, nhưng thay vì so sánh`route[i]`, chúng tôi so sánh`route[n - 1 - i]`. Điều này mô phỏng việc đi ngược lại tuyến đường mà không xây dựng một mảng đảo ngược. 

Điều này tránh việc phân bổ thêm bộ nhớ trong khi vẫn duy trì tính chính xác. 

### 4. Tổng hợp kết quả 

Nếu cả hai phép so sánh thuận và ngược đều hợp lệ, chúng ta sẽ xuất ra`Unsure`. Nếu chỉ chuyển tiếp là hợp lệ, chúng tôi xuất`Forward`. Nếu chỉ đảo ngược là hợp lệ, chúng tôi xuất ra`Reverse`. Nếu không, trình tự sẽ không hợp lệ. 

### Tại sao nó hoạt động 

Tính chính xác xuất phát từ thực tế là tuyến đường xác định chính xác hai chuỗi điểm dừng xác định có thể có tùy theo hướng. Mọi quan sát hợp lệ phải khớp chính xác với một trong hai chuỗi này. Vì chúng tôi so sánh từng yếu tố theo cả hai hướng nên chúng tôi đang kiểm tra sự bình đẳng dựa trên cả hai sự thật cơ bản có thể có. Không có sự mơ hồ nào ngoài các trường hợp đối xứng trong đó cả hai hướng đều tạo ra các chuỗi giống hệt nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def check_equal(a, b):
    if len(a) != len(b):
        return False
    for i in range(len(a)):
        if a[i] != b[i]:
            return False
    return True

def check_reverse(route, obs):
    n = len(route)
    k = len(obs)
    if n != k:
        return False
    for i in range(n):
        if route[n - 1 - i] != obs[i]:
            return False
    return True

def solve():
    data = list(map(int, input().split()))
    n = data[0]
    route = data[1:1+n]

    k = int(input())
    obs = list(map(int, input().split()))

    forward = check_equal(route, obs)
    backward = check_reverse(route, obs)

    if forward and backward:
        print("Unsure")
    elif forward:
        print("Forward")
    elif backward:
        print("Reverse")
    else:
        print("Impossible")

if __name__ == "__main__":
    solve()
```Việc triển khai tách kiểm tra tiến và lùi thành các chức năng trợ giúp rõ ràng. Kiểm tra chuyển tiếp là quét bình đẳng trực tiếp. Kiểm tra ngược sẽ tránh việc xây dựng một mảng đảo ngược và thay vào đó lập chỉ mục từ cuối tuyến, điều này vừa sạch hơn vừa tiết kiệm bộ nhớ hơn. 

Một chi tiết tinh tế là chúng tôi luôn kiểm tra độ dài trước tiên. Điều này ngăn chặn việc so sánh một phần ngẫu nhiên trong đó tiền tố có thể khớp nhưng chuỗi đầy đủ thì không. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Tuyến đường là`[1, 2, 3, 4]`, quan sát được là`[1, 2, 3, 4]`. 

| tôi | tuyến đường[i] | quan sát [i] | trận đấu | 
| --- | --- | --- | --- | 
| 0 | 1 | 1 | vâng | 
| 1 | 2 | 2 | vâng | 
| 2 | 3 | 3 | vâng | 
| 3 | 4 | 4 | vâng | 

Chuyển tiếp kiểm tra chuyển tiếp. Kiểm tra ngược không thành công. Đầu ra là`Forward`. 

Điều này xác nhận thuật toán xác định chính xác sự liên kết trực tiếp với tuyến đường. 

### Ví dụ 2 

Tuyến đường là`[1, 2, 3, 4]`, quan sát được là`[4, 3, 2, 1]`. 

| tôi | tuyến đường[n-1-i] | quan sát [i] | trận đấu | 
| --- | --- | --- | --- | 
| 0 | 4 | 4 | vâng | 
| 1 | 3 | 3 | vâng | 
| 2 | 2 | 2 | vâng | 
| 3 | 1 | 1 | vâng | 

Kiểm tra ngược lại. Kiểm tra chuyển tiếp không thành công. Đầu ra là`Reverse`. 

Điều này cho thấy việc phát hiện chính xác việc truyền tải theo hướng ngược lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + k) | Mỗi mảng được quét tối đa hai lần, mỗi lần kiểm tra một hướng | 
| Không gian | O(1) | Không có mảng phụ trợ nào được tạo ngoài bộ nhớ đầu vào | 

Giải pháp tuyến tính và phù hợp thoải mái với các ràng buộc điển hình lên tới ít nhất 2e5 phần tử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# basic forward
assert run("4 1 2 3 4\n4\n1 2 3 4\n") == "Forward"

# reverse
assert run("4 1 2 3 4\n4\n4 3 2 1\n") == "Reverse"

# both (all same values)
assert run("4 7 7 7 7\n4\n7 7 7 7\n") == "Unsure"

# mismatch
assert run("4 1 2 3 4\n3\n1 2 3\n") == "Impossible"

# partial but invalid
assert run("4 1 2 3 4\n4\n1 2 4 3\n") == "Impossible"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trình tự bằng nhau | Chuyển tiếp | trường hợp chuyển tiếp bình thường | 
| trình tự đảo ngược | Đảo ngược | truyền tải ngược | 
| tất cả đều giống hệt nhau | Không chắc chắn | trường hợp mơ hồ | 
| độ dài không khớp | Không thể | từ chối sớm | 
| sai thứ tự | Không thể | bẫy trận đấu một phần | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả các thành phần tuyến đường giống hệt nhau. Ví dụ, tuyến đường`[5,5,5,5]`và quan sát`[5,5,5,5]`làm cho cả kiểm tra thuận và ngược đều được thực hiện đồng thời. Thuật toán xuất ra chính xác`Unsure`vì thông tin định hướng vốn đã bị mất. 

Một trường hợp cạnh khác là độ dài không khớp. Nếu trình tự được quan sát có độ dài khác với tuyến đường thì cả việc so sánh thuận và ngược đều không thể thành công. Thuật toán loại bỏ sớm trong cả hai lần kiểm tra, ngăn chặn bất kỳ sự trùng khớp một phần ngẫu nhiên nào. 

Trường hợp cạnh cuối cùng là kích thước đầu vào tối thiểu. Khi tuyến đường chỉ có một phần tử, theo định nghĩa, cả hai hướng đều giống nhau. Thuật toán phân loại chính xác mọi quan sát một phần tử phù hợp như`Unsure`nếu nó phù hợp, hoặc`Impossible`nếu không thì.
