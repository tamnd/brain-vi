---
problem: 1036D
contest_id: 1036
problem_index: D
name: "Vasya and Arrays"
contest_name: "Educational Codeforces Round 50 (Rated for Div. 2)"
rating: 1600
tags: ["greedy", "two pointers"]
answer: passed_samples
verified: true
solve_time_s: 75
date: 2026-06-17
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a32a1e8-5644-83ec-ad41-fedccd51e290
---

# CF 1036D - Vasya và Mảng 

**Đánh giá:** 1600 
**Tags:** tham lam, hai con trỏ 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a32a1e8-5644-83ec-ad41-fedccd51e290 

--- 

## Giải pháp 

## Hiểu vấn đề 

Cho ta hai dãy số nguyên dương. Chúng ta được phép nén nhiều lần bất kỳ khối liền kề nào thành một số duy nhất bằng tổng của khối đó. Sau khi thực hiện các thao tác nén này một cách độc lập trên cả hai mảng, chúng tôi muốn các mảng kết quả trở nên giống hệt nhau và trong số tất cả các khả năng như vậy, chúng tôi muốn độ dài tối đa có thể có của các mảng bằng nhau cuối cùng. Nếu không có cách nào để làm cho chúng bằng nhau thì chúng tôi cho rằng không thể thực hiện được. 

Giải thích chính là chúng tôi đang phân vùng cả hai mảng thành các phân đoạn và mỗi phân đoạn trở thành một giá trị bằng tổng của nó. Mục tiêu là tìm một phân đoạn chung của cả hai mảng sao cho tổng phân đoạn tương ứng khớp với nhau, đồng thời tối đa hóa số lượng phân đoạn. 

Các ràng buộc cho phép mảng lên tới 300.000 phần tử. Bất kỳ giải pháp nào thử tất cả các phân vùng hoặc sử dụng phân tách theo cấp số nhân đều không thể thực hiện được ngay lập tức. Ngay cả các chiến lược hợp nhất bậc hai trên tất cả các phân đoạn cũng sẽ quá chậm trong trường hợp xấu nhất vì mỗi phần tử tham gia vào nhiều hợp nhất tiềm năng. 

Một vài hành vi cạnh có vấn đề. 

Nếu một mảng có một phần tử duy nhất và mảng kia có nhiều phần tử có tổng tổng khớp nhưng không thể phân chia thành các phần tổng khớp nhau, thì câu trả lời là không thể mặc dù tổng toàn cục khớp nhau. Ví dụ: A = [5], B = [2, 3] là hợp lệ và mang lại độ dài 1, nhưng A = [5], B = [1, 4] cũng hợp lệ, trong khi A = [5], B = [2, 2, 1] là không thể vì không có phân đoạn nào tạo ra các tổng liền kề phù hợp được căn chỉnh ở cả hai phía. 

Một trường hợp tinh tế khác là khi việc sáp nhập tham lam có vẻ mang lại lợi ích cục bộ nhưng lại phá vỡ sự liên kết trong tương lai. Ví dụ: nếu một mảng tích lũy quá nhiều trước khi mảng kia đạt cùng số tiền, thì việc căn chỉnh sau này sẽ không thể thực hiện được ngay cả khi tổng số tiền vẫn khớp. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử mọi cách chia cả hai mảng thành các phân đoạn liền kề và kiểm tra xem liệu chúng ta có thể khớp tổng phân đoạn theo cặp hay không. Số cách phân vùng một mảng có độ dài n là theo cấp số nhân, vì vậy điều này không thể thực hiện được ngay cả với n khoảng 30. 

Một cách nhìn có cấu trúc hơn là hãy tưởng tượng việc đi qua cả hai mảng cùng một lúc và tạo thành các phân đoạn một cách tham lam. Vì tất cả các giá trị đều dương nên tổng một phần sẽ tăng lên khi chúng ta mở rộng một phân đoạn. Tính đơn điệu này cho phép mô phỏng hai con trỏ: chúng tôi xây dựng tổng phân đoạn hiện tại trên A và B, và bất cứ khi nào một bên nhỏ hơn, chúng tôi sẽ nâng cao nó; khi chúng bằng nhau, chúng tôi cam kết một phân đoạn và đặt lại. 

Quan sát quan trọng là chúng ta không bao giờ cần phải xem xét lại các quyết định trước đó. Vì các giá trị là dương nên khi tổng tiền tố khớp nhau, phân đoạn đó sẽ bị buộc phải đưa vào bất kỳ phân vùng tối ưu hợp lệ nào để giúp cả hai bên được đồng bộ hóa. Nếu chúng tôi cố gắng trì hoãn việc hợp nhất, chúng tôi sẽ chỉ giảm số lượng phân đoạn chứ không tăng số lượng đó, vì việc hợp nhất trước đó mang lại nhiều ranh giới thẳng hàng hơn. 

Điều này làm giảm vấn đề quét cả hai mảng một lần và sắp xếp các tổng phân đoạn một cách tham lam. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân vùng Brute Force | Hàm mũ | O(1) | Quá chậm | 
| Khớp tổng tham lam hai con trỏ | O(n + m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai tổng chạy, một cho mỗi mảng và hai con trỏ. Mỗi bước mở rộng cạnh có tổng nhỏ hơn cho đến khi cả hai tổng khớp nhau, sau đó chúng tôi ghi lại một đoạn.

1. Khởi tạo con trỏ i và j tại 0 và chạy tổng sa và sb tại 0. 
2. Khi cả hai mảng đều chưa hết, hãy thêm A[i] vào sa nếu sa <= sb, nếu không thì thêm B[j] vào sb. Điều này khiến chúng ta luôn cố gắng cân bằng hai tổng riêng. 
3. Khi sa bằng sb, chúng ta đã tìm được một đoạn phù hợp. Chúng tôi tăng câu trả lời và đặt lại cả hai tổng thành 0. 
4. Nếu một con trỏ đến cuối mà con trỏ còn lại vẫn còn tổng thì phải đảm bảo bên còn lại cũng được sử dụng hết; nếu không thì việc kết hợp là không thể. 
5. Sau vòng lặp, nếu cả hai tổng đều bằng 0 và cả hai con trỏ đều ở cuối, trả về số phân đoạn trùng khớp; nếu không thì trả về -1. 

Lý do chúng tôi luôn mở rộng số tiền nhỏ hơn là vì bất kỳ sự căn chỉnh hợp lệ nào cuối cùng cũng phải cân bằng tổng số và việc trì hoãn việc mở rộng ở phía nhỏ hơn sẽ chỉ trì hoãn sự bình đẳng mà không cải thiện số lượng phân đoạn. 

### Tại sao nó hoạt động 

Ở mỗi bước, thuật toán duy trì rằng phân đoạn một phần hiện tại trên A và B đại diện cho phần chưa khớp tiếp theo của phân vùng hợp lệ giả định. Vì tất cả các số đều dương nên tổng các phân số tăng đơn điệu, do đó chỉ có thể đạt được sự bằng nhau bằng cách mở rộng cạnh nhỏ hơn. Sau khi đạt được sự bình đẳng, ranh giới đó sẽ bị ép buộc trong bất kỳ phân vùng nào nhằm tối đa hóa số lượng phân đoạn, vì việc hợp nhất qua một điểm mà cả hai bên đã khớp sẽ làm giảm nghiêm trọng số lượng phân đoạn mà không cho phép bất kỳ căn chỉnh mới nào sau này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    m = int(input())
    b = list(map(int, input().split()))
    
    i = j = 0
    sa = sb = 0
    ans = 0
    
    while i < n or j < m:
        if sa == sb:
            if sa != 0:
                ans += 1
                sa = sb = 0
        
        if sa <= sb:
            if i < n:
                sa += a[i]
                i += 1
            else:
                sa = float('inf')
        else:
            if j < m:
                sb += b[j]
                j += 1
            else:
                sb = float('inf')
    
    if sa == sb:
        if sa != 0:
            ans += 1
    else:
        print(-1)
        return
    
    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai giữ hai con trỏ quét từng mảng chính xác một lần. Quyết định cốt lõi được kiểm soát bằng cách so sánh số tiền tích lũy hiện tại. Khi một cạnh nhỏ hơn hoặc bằng thì nó được kéo dài. Điều này đảm bảo rằng cả hai bên đều tiến lên một cách đồng bộ mà không bị lùi lại. 

Việc sử dụng một giá trị trọng điểm như vô cùng khi một mảng đã hết sẽ ngăn chặn các vòng lặp vô hạn và buộc phải phát hiện sự không khớp. Việc kiểm tra cuối cùng đảm bảo rằng không còn một phần tổng chưa khớp nào. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

A = [11, 2, 3, 5, 7] 

B = [11, 7, 3, 7] 

Chúng tôi theo dõi số tiền như sau. 

| Bước | tôi | j | sa | sb | Hành động | Phân đoạn | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 11 | 0 | lấy A | | 
| 2 | 0 | 0 | 11 | 11 | lấy B | [11] | 
| 3 | 1 | 1 | 2 | 7 | lấy A | [11] | 
| 4 | 2 | 1 | 5 | 7 | lấy A | [11] | 
| 5 | 3 | 1 | 10 | 7 | lấy B | [11] | 
| 6 | 3 | 2 | 10 | 10 | lấy B | [11, 10] | 
| 7 | 4 | 3 | 7 | 7 | cả hai thiết lập lại | [11, 10, 7] | 

Câu trả lời cuối cùng là 3 đoạn. 

Dấu vết này cho thấy rằng các điểm đẳng thức xảy ra chính xác khi tổng cả hai phân đoạn tiền tố đều căn chỉnh và mỗi căn chỉnh như vậy tạo ra một phân đoạn. 

### Ví dụ 2 

A = [1, 2, 3, 3] 

B = [3, 3, 3] 

| Bước | tôi | j | sa | sb | Hành động | Phân đoạn | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 1 | 0 | A | | 
| 2 | 1 | 0 | 3 | 0 | A | | 
| 3 | 1 | 0 | 3 | 3 | B | [3] | 
| 3 | 2 | 1 | 0 | 0 | đặt lại | [3] | 
| 4 | 2 | 1 | 3 | 0 | A | | 
| 5 | 2 | 2 | 3 | 3 | B | [3, 3] | 
| 6 | 3 | 3 | 0 | 0 | đặt lại | [3, 3] | 
| 7 | 3 | 3 | 3 | 0 | A | | 
| 8 | 4 | 3 | 6 | 0 | Một kết thúc không khớp | thất bại | 

Điều này thể hiện trường hợp lỗi trong đó tổng số tiền có thể khớp cục bộ trong các phân đoạn nhưng phần còn lại cuối cùng không thể căn chỉnh được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | mỗi phần tử được con trỏ truy cập một lần | 
| Không gian | O(1) | chỉ số tiền đang chạy và bộ đếm được lưu trữ | 

Quét tuyến tính phù hợp thoải mái trong giới hạn cho mảng lên tới 300.000 phần tử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue().strip()
    sys.stdout = old_stdout
    return out

# provided sample
assert run("""5
11 2 3 5 7
4
11 7 3 7
""") == "3"

# equal single element
assert run("""1
5
1
5
""") == "1"

# impossible mismatch
assert run("""2
1 2
2
2 2
""") == "-1"

# multiple merges required
assert run("""4
1 1 2 2
2
2 4
""") == "2"

# large equal arrays
assert run("""3
1 1 1
3
1 1 1
""") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử bằng nhau | 1 | trận đấu tầm thường | 
| số tiền không khớp | -1 | trường hợp bất khả thi | 
| sáp nhập nhóm | 2 | phân đoạn chính xác | 
| tất cả những cái | 3 | chia tối đa | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi một mảng hết sớm hơn nhưng vẫn có tổng một phần khác 0. Trong tình huống đó, thuật toán buộc phải không khớp vì không còn phần tử nào tồn tại để cân bằng nó. Ví dụ A = [1, 2], B = [1, 1, 1]. Quá trình căn chỉnh 1 đầu tiên, sau đó A tích lũy 2 trong khi B tích lũy 1, 1 và chỉ các kết quả khớp sau đó, nhưng nếu thứ tự khác nhau thì việc cạn kiệt sẽ ngăn cản sự bình đẳng cuối cùng. 

Một trường hợp cạnh khác xảy ra khi đạt được sự bình đẳng chính xác ở cuối cả hai mảng. Kiểm tra cuối cùng đảm bảo rằng phân đoạn cuối cùng được tính ngay cả khi không có lần lặp nào nữa xảy ra sau khi vòng lặp kết thúc.
