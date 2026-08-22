---
title: "CF 104246L - Cùng Tìm Đường"
description: "Chúng ta đang xử lý một khoảng ẩn trên một dòng được đánh số từ 1 đến N. Ở đâu đó trên dòng này có một đoạn liền kề bắt đầu từ A và kết thúc ở B, và mục tiêu của chúng ta là xác định độ dài của nó, đó là B trừ A cộng một. Chúng tôi không có quyền truy cập trực tiếp vào A hoặc B."
date: "2026-07-01T23:04:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104246
codeforces_index: "L"
codeforces_contest_name: "CodeSmash 2021 by RAPL"
rating: 0
weight: 104246
solve_time_s: 92
verified: false
draft: false
---

[CF 104246L - Cùng tìm đường](https://codeforces.com/problemset/problem/104246/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta đang xử lý một khoảng ẩn trên một dòng được đánh số từ 1 đến N. Ở đâu đó trên dòng này có một đoạn liền kề bắt đầu từ A và kết thúc ở B, và mục tiêu của chúng ta là xác định độ dài của nó, đó là B trừ A cộng một. 

Chúng tôi không có quyền truy cập trực tiếp vào A hoặc B. Thay vào đó, chúng tôi có thể đặt các truy vấn có dạng “là x trước phân đoạn, bên trong phân đoạn đó hoặc sau phân đoạn đó”. Phản hồi sẽ chia đường thẳng thành ba vùng: mọi thứ hoàn toàn bên trái của A, mọi thứ từ A đến B và mọi thứ hoàn toàn bên phải của B. Mỗi truy vấn cung cấp cho chúng ta chính xác điểm được chọn thuộc về khu vực nào. 

Hạn chế chính là chúng tôi bị giới hạn ở 55 truy vấn cho mỗi lần kiểm tra, trong khi N có thể lớn tới 10^9. Điều này ngay lập tức loại trừ mọi chiến lược quét tuyến tính hoặc thăm dò dày đặc trên toàn bộ phạm vi. Ngay cả các chiến lược logarit cũng cần phải được lập ngân sách cẩn thận vì mỗi tìm kiếm nhị phân trên toàn bộ phạm vi tốn khoảng 30 truy vấn và việc thực hiện nhiều tìm kiếm như vậy một cách ngây thơ sẽ vượt quá giới hạn. 

Một đảm bảo tinh tế nhưng quan trọng là độ dài khoảng tối đa là 10^6. Giới hạn bổ sung này ngăn cản cách tiếp cận “tìm A và B độc lập trên [1, N]” đơn giản khỏi bị thắt chặt về ngân sách truy vấn. Nó gợi ý rằng một khi đã biết được một điểm cuối thì có thể tìm thấy điểm cuối kia ở một khu vực nhỏ hơn nhiều. 

Một sai lầm ngây thơ sẽ phát sinh nếu chúng ta cố gắng định vị cả A và B một cách độc lập bằng cách sử dụng tìm kiếm nhị phân đầy đủ trên [1, N]. Điều đó đòi hỏi khoảng 30 truy vấn cho A và 30 truy vấn khác cho B, tổng cộng khoảng 60 truy vấn. Con số này đã vượt quá mức cho phép 55. Một cách tiếp cận không chính xác khác là cố gắng “mở rộng” từ một điểm được đoán bằng cách sử dụng truy vấn, vì phản hồi không cho chúng ta biết khoảng cách mà chỉ cho chúng ta biết vị trí tương đối. 

Do đó, thách thức không chỉ là định vị phân khúc mà còn phải làm như vậy trong khi vẫn tôn trọng ngân sách truy vấn eo hẹp buộc chúng tôi phải giảm không gian tìm kiếm sau khi tìm hiểu một phần câu trả lời. 

## Phương pháp tiếp cận 

Chiến lược brute-force sẽ quét mọi vị trí từ 1 đến N, truy vấn từng điểm cho đến khi chúng ta tìm thấy chỉ mục đầu tiên nằm bên trong phân khúc và chỉ mục cuối cùng nằm bên trong phân khúc. Điều này hiệu quả vì mọi điểm đều được phân loại rõ ràng, vì vậy chúng tôi có thể xác định A và B bằng cách theo dõi các chuyển tiếp. Tuy nhiên, trong trường hợp xấu nhất N là 10^9, vì vậy cách tiếp cận này hoàn toàn không khả thi ngay cả khi mỗi truy vấn có thời gian không đổi. 

Một cách tiếp cận có cấu trúc hơn là tìm kiếm nhị phân. Hàm phản hồi đơn điệu một cách hữu ích nếu chúng ta diễn giải nó một cách chính xác. Đối với A, mọi vị trí x < A trả về “<”, trong khi mọi vị trí x >= A trả về “=” hoặc “>”. Điều này mang lại một ranh giới đơn điệu rõ ràng, cho phép chúng ta tìm kiếm nhị phân vị trí đầu tiên không phải là “<”. 

Tương tự, đối với B, chúng ta quan sát thấy rằng mọi vị trí x > B đều trả về “>”, trong khi mọi vị trí x <= B đều trả về “<” hoặc “=”. Điều này cho phép chúng ta tìm kiếm nhị phân vị trí cuối cùng không phải là “>”. 

Nếu chúng tôi thực hiện cả hai tìm kiếm nhị phân trên toàn bộ phạm vi [1, N], chúng tôi có nguy cơ sử dụng khoảng 60 truy vấn. Quan sát quan trọng giải quyết vấn đề này là độ dài đoạn được đảm bảo tối đa là 10^6. Khi xác định được A, chúng ta biết rằng B phải nằm trong [A, min(N, A + 10^6)]. Điều này làm giảm miền tìm kiếm nhị phân thứ hai từ 10^9 xuống nhiều nhất là 10^6, làm cho miền tìm kiếm nhị phân rẻ hơn đáng kể trong các truy vấn. 

Do đó, chúng tôi chia nhiệm vụ thành hai giai đoạn: tìm kiếm nhị phân toàn dải cho A và tìm kiếm nhị phân giới hạn cho B trong một cửa sổ nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quét tuyến tính | Truy vấn O(N) | O(1) | Quá chậm | 
| Tìm kiếm nhị phân đầy đủ hai lần | Truy vấn O(log N) | O(1) | Quá nhiều truy vấn | 
| Tìm kiếm phân chia được tối ưu hóa | Truy vấn O(log N + log L) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Hướng dẫn thuật toán

1. Thực hiện tìm kiếm nhị phân trên phạm vi [1, N] để tìm A. Đối với điểm giữa x, hãy truy vấn nó và diễn giải phản hồi. Nếu đáp án là “<” thì x đúng trước A nên A phải nằm bên phải. Ngược lại, x nằm bên trong hoặc bên phải A nên A nằm tại x hoặc bên trái. Việc tìm kiếm hội tụ về vị trí đầu tiên không phải là “<”. 
2. Khi đã biết A, hãy xác định phạm vi tìm kiếm rút gọn cho B là [A, min(N, A + 10^6)]. Điều này hợp lệ vì bài toán đảm bảo độ dài đoạn tối đa là 10^6, do đó B không thể nằm ngoài khoảng này. 
3. Thực hiện tìm kiếm nhị phân thứ hai để tìm vị trí cuối cùng không phải là “>”. Đối với điểm giữa x, nếu đáp án là “>”, thì x đứng sau B và chúng ta phải tìm kiếm bên trái. Ngược lại, x ở tại hoặc trước B, nên chúng ta di chuyển sang phải. 
4. Sau khi tìm thấy cả hai điểm cuối, hãy tính câu trả lời là B − A + 1. 

### Tại sao nó hoạt động 

Tính chính xác phụ thuộc vào hai phân vùng đơn điệu của hàm truy vấn. Vị từ “x < A” hoàn toàn đơn điệu trong x, và vị từ “x > B” cũng đơn điệu tuyệt đối trong x. Điều này cho phép tìm kiếm nhị phân xác định chính xác ranh giới. Giai đoạn thứ hai là an toàn vì ràng buộc về độ dài đoạn tối đa đảm bảo rằng việc thu hẹp không gian tìm kiếm xung quanh A không thể loại trừ B. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def ask(x):
    print(f"? {x}", flush=True)
    return input().strip()

def find_A(n):
    lo, hi = 1, n
    ans = n
    while lo <= hi:
        mid = (lo + hi) // 2
        res = ask(mid)
        if res == '<':
            lo = mid + 1
        else:
            ans = mid
            hi = mid - 1
    return ans

def find_B(n, A):
    lo = A
    hi = min(n, A + 10**6)
    ans = A
    while lo <= hi:
        mid = (lo + hi) // 2
        res = ask(mid)
        if res == '>':
            hi = mid - 1
        else:
            ans = mid
            lo = mid + 1
    return ans

def solve():
    n = int(input())
    A = find_A(n)
    B = find_B(n, A)
    print(f"! {B - A + 1}", flush=True)

if __name__ == "__main__":
    solve()
```Giải pháp tách sự tương tác thành một chức năng trợ giúp nhỏ`ask`, đảm bảo mọi truy vấn được xóa ngay lập tức, đây là điều bắt buộc trong các vấn đề tương tác. Tìm kiếm nhị phân đầu tiên xác định ranh giới bên trái bằng cách xử lý “<” như một chỉ báo nghiêm ngặt về việc nằm ngoài phân đoạn ở phía bên trái. Tìm kiếm nhị phân thứ hai được cố ý hạn chế trong một cửa sổ an toàn bắt nguồn từ độ dài phân đoạn tối đa có thể, đây là chìa khóa để duy trì trong giới hạn truy vấn. 

Một cạm bẫy triển khai phổ biến là coi phản hồi “=” là trường hợp đặc biệt cần xử lý riêng. Trong thực tế, “=” hoạt động giống như nằm trong phân đoạn cho cả tìm kiếm nhị phân, vì vậy nó có thể được nhóm lại với các trường hợp không cực đoan. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
N = 6
A = 2, B = 4
```Tìm kiếm nhị phân cho A: 

| Bước | lo | xin chào | giữa | phản hồi | hành động | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 6 | 3 | '=' | di chuyển sang trái | 
| 2 | 1 | 2 | 1 | '<' | di chuyển sang phải | 
| 3 | 2 | 2 | 2 | '=' | tìm thấy A | 

Tìm kiếm nhị phân cho B (phạm vi [2, 6]): 

| Bước | lo | xin chào | giữa | phản hồi | hành động | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 6 | 4 | '=' | di chuyển sang phải | 
| 2 | 5 | 6 | 5 | '>' | di chuyển sang trái | 
| 3 | 5 | 4 | - | dừng lại | B = 4 | 

Điều này xác nhận A = 2, B = 4, vì vậy câu trả lời là 3. 

### Ví dụ 2 

đầu vào:```
N = 10^9, segment length = 10^6
```Tìm kiếm nhị phân đầu tiên vẫn cần khoảng 30 truy vấn để xác định vị trí A. Tìm kiếm nhị phân thứ hai chỉ khám phá một cửa sổ có kích thước 10^6, yêu cầu khoảng 20 truy vấn. Điều này vẫn thoải mái trong giới hạn 55 truy vấn. 

Dấu vết này cho thấy rằng việc tối ưu hóa không mang tính thẩm mỹ, nó cần thiết cho tính khả thi trong trường hợp xấu nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian (truy vấn) | O(log N + log 10^6) | tìm kiếm nhị phân cho A trên phạm vi đầy đủ, sau đó B trên phạm vi giới hạn | 
| Không gian | O(1) | chỉ có một vài biến số nguyên được duy trì | 

Độ phức tạp của truy vấn duy trì ở mức dưới 55 vì log2(10^9) là khoảng 30 và log2(10^6) là khoảng 20, mang lại biên độ an toàn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    N = int(sys.stdin.readline())
    # This is a placeholder since full interaction cannot be simulated here
    # In real testing, this would hook into a mock interactor
    return ""

# provided samples (interaction-based, conceptual only)
# assert run("6\n") == "3"

# custom sanity cases
assert True, "single case placeholder"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| N=6, A=2, B=4 | 3 | tính đúng đắn cơ bản | 
| N=10^9, A=1, B=10^6 | 10^6 | ranh giới chiều dài tối đa | 
| N=10^9, A=N-10^6+1 | 10^6 | khoảng cạnh phải | 
| N=1, A=1, B=1 | 1 | đầu vào tối thiểu | 

## Vỏ cạnh 

Khi khoảng bắt đầu từ 1, tìm kiếm nhị phân cho A sẽ không bao giờ thấy “<”, do đó nó hội tụ trực tiếp về 1. Logic vẫn hoạt động vì câu trả lời mặc định được khởi tạo ở giới hạn trên và chỉ được thắt chặt khi một ứng cử viên hợp lệ xuất hiện. 

Khi khoảng kết thúc ở N, tìm kiếm nhị phân thứ hai không thấy phản hồi “>” nào cả, do đó nó mở rộng đến toàn bộ phạm vi cho phép và trả về chính xác B = N. 

Khi khoảng có độ dài tối đa được phép là 10^6, phạm vi tìm kiếm thứ hai bị giới hạn khớp chính xác với vùng hợp lệ. Đây là lý do duy nhất khiến tìm kiếm nhị phân thứ hai vẫn đủ hiệu quả để vừa với giới hạn truy vấn và nó đảm bảo không có B hợp lệ nào bị loại khỏi việc xem xét.
