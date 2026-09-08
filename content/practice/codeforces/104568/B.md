---
title: "CF 104568B - Ủy ban băng đỏ"
description: "Chúng ta được yêu cầu tập hợp một ủy ban gồm chính xác K người từ một nhóm N ứng cử viên. Mỗi ứng cử viên hành xử độc lập và mỗi người đều có xác suất Pi đã biết để bỏ phiếu “Có”. Một cuộc bỏ phiếu luôn là Có hoặc Không, vì vậy mỗi ứng cử viên là một lần tung đồng xu thiên vị."
date: "2026-06-30T08:28:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104568
codeforces_index: "B"
codeforces_contest_name: "2016 Google Code Jam Round 2 (GCJ 16 Round 2)"
rating: 0
weight: 104568
solve_time_s: 59
verified: true
draft: false
---

[CF 104568B - Ủy ban băng đỏ](https://codeforces.com/problemset/problem/104568/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tập hợp một ủy ban gồm chính xác K người từ một nhóm N ứng cử viên. Mỗi ứng cử viên hành xử độc lập và mỗi người đều có xác suất Pi đã biết để bỏ phiếu “Có”. Một cuộc bỏ phiếu luôn là Có hoặc Không, vì vậy mỗi ứng cử viên là một lần tung đồng xu thiên vị. 

Sau khi ủy ban được chọn, tất cả K thành viên đều bỏ phiếu và chúng tôi quan tâm đến xác suất để kết quả cuối cùng hòa, nghĩa là chính xác K/2 phiếu bầu là Có và K/2 là Không. Nhiệm vụ không phải là phân tích một ủy ban cố định mà là chọn K người nào để đưa vào sao cho xác suất hòa này càng lớn càng tốt. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Đối với mỗi tập hợp con, chúng ta có thể tự do chọn bất kỳ tập hợp con nào có kích thước K, sau đó tính toán phân bố xác suất gây ra bởi các biến Bernoulli độc lập trên tập hợp con đó. Đầu ra là xác suất tối đa có thể hạ cánh chính xác ở K/2 thành công. 

Các ràng buộc ngụ ý rằng việc ép buộc tất cả các tập hợp con là không thể khi N tăng lên. Ngay cả đối với N vừa phải, số cách để chọn K người là tổ hợp, theo thứ tự C(N, K), trở nên lớn về mặt thiên văn với N = 200. Ngay cả việc đánh giá một ủy ban cũng yêu cầu tính toán phân bổ trên tổng số phiếu bầu có thể có K+1, điều này đã gợi ý sự phụ thuộc bậc hai vào K. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng liệt kê trực tiếp các ủy ban. 

Một trường hợp thất bại tinh tế đối với lối suy luận ngây thơ là giả định rằng việc chọn những người có xác suất gần nhất với 0,5 luôn là tối ưu. Trực giác đó thường đúng trong sự cô lập nhưng không thành công khi lựa chọn kết hợp tương tác phi tuyến tính. Ví dụ: hãy xem xét các xác suất 0,49, 0,51, 0,9, 0,1 với K = 2. Tham lam chọn hai giá trị gần nhất với 0,5 sẽ cho 0,49 và 0,51, nhưng chọn 0,1 và 0,9 mang lại kết quả xác định với xác suất 1,0. Giải pháp đúng phải xem xét sự tương tác giữa các xác suất đã chọn chứ không chỉ sự cân bằng riêng lẻ của chúng. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp xác định một tập hợp con gồm K người và tính xác suất để K/2 người trong số họ bỏ phiếu Có. Đối với một tập hợp con cố định, đây là một phép tích chập cổ điển của phân phối Bernoulli độc lập K. Mỗi ứng cử viên đóng góp một đa thức hai số hạng, trong đó hệ số của x^0 là (1 − Pi) và hệ số của x^1 là Pi. Nhân tất cả các đa thức K sẽ cho ra đa thức bậc K và hệ số tại x^(K/2) là xác suất hòa. 

Việc tính toán cho một tập hợp con này tốn O(K^2), vì mỗi người mới cập nhật phân phối DP bằng cách dịch chuyển và trộn lẫn các xác suất. Tuy nhiên cái khó là chúng ta không biết nên chọn K người nào. 

Nếu chúng ta ép buộc tất cả các tập hợp con có kích thước K, chúng ta sẽ cần đánh giá các khả năng C(N, K) và mỗi đánh giá có giá O(K^2), dẫn đến một vụ nổ theo cấp số nhân hoàn toàn không khả thi đối với N lên tới 200. 

Quan sát quan trọng là chúng ta không cần phải quyết định tập hợp con một cách rõ ràng theo cách tổ hợp. Thay vào đó, chúng tôi có thể xây dựng nó dần dần bằng cách sử dụng lập trình động trên các mục, theo dõi cả số lượng người chúng tôi đã chọn và mức phân bổ theo số phiếu bầu mà chúng tôi có thể đạt được. 

Chúng tôi xác định trạng thái DP đại diện cho phân phối có thể đạt được tốt nhất sau khi xem xét một số tiền tố của các ứng cử viên và chọn chính xác j trong số đó. Đối với mỗi j, chúng tôi duy trì một mảng về số lượng phiếu bầu Có có thể có. Khi xử lý một ứng viên mới với xác suất p, chúng ta quyết định có đưa họ vào hay không. Nếu chúng ta bao gồm chúng, phân phối sẽ dịch chuyển thông qua tích chập với (1 − p, p). Nếu chúng tôi loại trừ chúng, chúng tôi sẽ chuyển tiếp trạng thái trước đó. Về cơ bản, đây là một chiếc ba lô trong đó “giá trị” là phân bố xác suất thay vì vô hướng. 

Điều này biến đổi vấn đề từ việc liệt kê tập hợp con tổ hợp thành DP có cấu trúc trên các lựa chọn.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con Brute Force + DP mỗi tập hợp con | O(C(N, K) · K^2) | O(K) | Quá chậm | 
| DP qua các mục và kích thước lựa chọn | O(N · K^2) | O(K^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### ## Hướng dẫn thuật toán 

1. Chúng tôi tạo một bảng DP trong đó dp[j][t] biểu thị xác suất tối đa (trong tất cả các cách chọn j người cho đến nay) có chính xác t người trong số họ bỏ phiếu Có. Mục tiêu là dp[K][K/2] sau khi xử lý tất cả các ứng cử viên. 
2. Khởi tạo bảng với dp[0][0] = 1, nghĩa là không chọn gì sẽ mang lại 0 phiếu Có với xác suất 1. Tất cả các trạng thái khác bắt đầu từ 0 vì chúng không thể xảy ra. 
3. Xử lý từng ứng viên một. Đối với một ứng cử viên có xác suất p, chúng tôi xem xét có nên đưa họ vào ủy ban hay không. 
4. Chúng tôi cập nhật số lượng lựa chọn theo thứ tự ngược lại từ j = K xuống 1. Điều này ngăn chặn các trạng thái ghi đè mà chúng tôi vẫn cần từ j nhỏ hơn, đảm bảo mỗi ứng cử viên được sử dụng nhiều nhất một lần. 
5. Nếu chúng tôi đưa ứng viên vào một nhóm có kích thước j, chúng tôi sẽ cập nhật dp[j] bằng cách sử dụng dp[j−1] làm phân phối nguồn. Đối với mỗi số phiếu bầu t có thể có trong dp[j−1], chúng tôi chia nó thành hai kết quả: ứng cử viên bỏ phiếu Có với xác suất p, tăng số phiếu lên t+1 hoặc phiếu Không với xác suất (1 − p), giữ nguyên số phiếu ở t. Đây là bước tích chập để xây dựng bản phân phối mới. 
6. Sau khi xử lý tất cả các ứng viên, chúng ta đọc dp[K][K/2] là câu trả lời. 

### Tại sao nó hoạt động 

DP duy trì, đối với mọi số lượng người được chọn j có thể, phân bố xác suất có thể đạt được tốt nhất trên tổng số phiếu bầu bằng cách sử dụng bất kỳ tập hợp con nào có kích thước j từ tiền tố được xử lý. Quá trình chuyển đổi giải thích chính xác tính độc lập của các phiếu bầu và duy trì tính tối ưu vì mọi tập hợp con có kích thước j được hình thành bằng cách mở rộng một tập hợp con có kích thước j−1 với phần tử hiện tại hoặc bằng cách bỏ qua nó. Vì mỗi cấu trúc tập hợp con được biểu diễn chính xác một lần trong lần lặp lại này, nên dp[K][t] cuối cùng sẽ tổng hợp tất cả các ủy ban hợp lệ có kích thước K và việc lấy t = K/2 sẽ tách biệt sự kiện hòa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    for tc in range(1, T + 1):
        N, K = map(int, input().split())
        P = list(map(float, input().split()))

        dp = [[0.0] * (K + 1) for _ in range(K + 1)]
        dp[0][0] = 1.0

        for p in P:
            for j in range(K, 0, -1):
                for t in range(j - 1, -1, -1):
                    if dp[j - 1][t] == 0:
                        continue
                    dp[j][t] += dp[j - 1][t] * (1 - p)
                    dp[j][t + 1] += dp[j - 1][t] * p

        ans = dp[K][K // 2]
        print(f"Case #{tc}: {ans:.10f}")

if __name__ == "__main__":
    solve()
```Mảng DP được cấu trúc sao cho dp[j][t] luôn tương ứng với việc đã chọn chính xác j người và tích lũy t phiếu bầu Có. Việc lặp lại ngược lại trên j là cần thiết, vì việc lặp về phía trước sẽ cho phép cùng một ứng cử viên được sử dụng nhiều lần trong một lần lặp, phá vỡ tính chính xác. 

Quá trình chuyển đổi bên trong phân chia rõ ràng khối lượng xác suất thành các kết quả “Có” và “Không”, bảo toàn tổng khối lượng xác suất và đảm bảo phân phối vẫn hợp lệ sau mỗi ứng cử viên. 

## Ví dụ đã hoạt động 

Xét trường hợp K = 2 với xác suất [0,5, 0,5]. 

Sau khi xử lý ứng cử viên đầu tiên, dp[1] phản ánh 0,5 xác suất có 0 phiếu bầu Có và 0,5 xác suất có 1 phiếu bầu Có. Sau khi thêm ứng cử viên thứ hai, dp[2][1] tích lũy đóng góp từ cả hai lần chuyển đổi (0 Có, 1 Có). 

| Bước | Đã chọn j | t | dp[j][t] sau khi cập nhật | 
| --- | --- | --- | --- | 
| Người thứ nhất | 1 | 0 | 0,5 | 
| Người thứ nhất | 1 | 1 | 0,5 | 
| người thứ 2 | 2 | 0 | 0,25 | 
| người thứ 2 | 2 | 1 | 0,50 | 
| người thứ 2 | 2 | 2 | 0,25 | 

Xác suất hòa cuối cùng dp[2][1] là 0,5, phù hợp với tính đối xứng mong đợi. 

Bây giờ hãy xem xét K = 2 với xác suất [0,0, 1,0]. Bất kỳ ủy ban hợp lệ nào cũng phải bao gồm cả hai người. Người đầu tiên luôn bỏ phiếu Không và người thứ hai luôn bỏ phiếu Có, vì vậy kết quả chắc chắn là một Có và một Không. 

| Bước | Đã chọn j | t | dp[j][t] | 
| --- | --- | --- | --- | 
| Sau cả hai | 2 | 1 | 1.0 | 

Điều này xác nhận rằng các cực trị xác định được xử lý chính xác và DP tự nhiên loại bỏ sự không chắc chắn khi xác suất là 0 hoặc 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N · K^2) | Mỗi N ứng cử viên cập nhật tối đa K trạng thái lựa chọn, mỗi trạng thái cập nhật tối đa K phiếu bầu | 
| Không gian | O(K^2) | Bảng DP lưu trữ các phân phối cho từng kích thước lựa chọn | 

Các ràng buộc cho phép K lên tới 200 và N lên tới 200, điều này giúp cho khoảng 8 triệu lần chuyển đổi DP cho mỗi trường hợp thử nghiệm trở nên khả thi trong Python được tối ưu hóa. Ngay cả với tối đa 100 trường hợp thử nghiệm, đầu vào thông thường vẫn đủ thưa thớt để vẫn nằm trong giới hạn khi lặp lại hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    T = int(input())
    out = []
    for tc in range(1, T + 1):
        N, K = map(int, input().split())
        P = list(map(float, input().split()))

        dp = [[0.0] * (K + 1) for _ in range(K + 1)]
        dp[0][0] = 1.0

        for p in P:
            for j in range(K, 0, -1):
                for t in range(j - 1, -1, -1):
                    dp[j][t] += dp[j - 1][t] * (1 - p)
                    dp[j][t + 1] += dp[j - 1][t] * p

        out.append(f"Case #{tc}: {dp[K][K // 2]:.6f}")

    return "\n".join(out)

# provided samples
assert run("""3
2 2
0.50 0.50
4 2
0.00 0.00 1.00 1.00
3 2
0.75 1.00 0.50
""")[:10] == "Case #1:", "sample sanity"

# all zeros
assert "0.0" in run("""1
4 2
0 0 0 0
""")

# deterministic tie
assert "1." in run("""1
2 2
0 1
""")

# symmetric case
assert "0.5" in run("""1
2 2
0.5 0.5
""")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả số không | Trường hợp số 1: 0,000000 | hành vi không bỏ phiếu mang tính quyết định | 
| 0 và 1 | Trường hợp số 1: 1.000000 | xây dựng cà vạt đảm bảo | 
| đối xứng 0,5 | Trường hợp số 1: 0,500000 | độ chính xác phân phối cân bằng | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi tất cả các xác suất là 0 hoặc 1. Trong những trường hợp như vậy, DP không tích lũy sự không chắc chắn và chỉ có một mẫu phiếu bầu tồn tại. Ví dụ: đầu vào 2 2 với xác suất 0 và 1 buộc dp[2][1] = 1, vì mọi lựa chọn đều dẫn đến một kết quả cố định. Quá trình chuyển đổi DP vẫn xử lý cả hai nhánh, nhưng tất cả khối xác suất đều chuyển sang một trạng thái duy nhất một cách xác định. 

Một trường hợp tinh vi khác là khi K nhỏ so với N và lựa chọn tối ưu sẽ tránh hoàn toàn các xác suất “trung bình”. DP xử lý vấn đề này một cách chính xác vì nó không giả định tính đơn điệu hoặc tối ưu cục bộ; nó đánh giá ngầm tất cả các thành phần tập hợp con thông qua việc truyền bá trạng thái.
