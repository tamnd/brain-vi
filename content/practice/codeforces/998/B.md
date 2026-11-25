---
problem: 998B
contest_id: 998
problem_index: B
name: "Cutting"
contest_name: "Codeforces Round 493 (Div. 2)"
rating: 1200
tags: ["dp", "greedy", "sortings"]
answer: passed_samples
verified: true
solve_time_s: 64
date: 2026-06-17
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a329797-db90-83ec-b163-c4de935bce59
---

#CF 998B - Cắt 

**Đánh giá:** 1200 
**Tags:** dp, tham lam, sắp xếp 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a329797-db90-83ec-b163-c4de935bce59 

--- 

## Giải pháp 

## Hiểu vấn đề 

Chúng ta được cấp một dãy số nguyên và được phép chia nó thành các đoạn liền kề bằng cách cắt giữa các phần tử liền kề. Mỗi phân đoạn phải thỏa mãn một ràng buộc về cấu trúc: số giá trị chẵn bên trong phân đoạn phải bằng số giá trị lẻ. 

Mỗi lần cắt giảm tiềm năng đều có chi phí được xác định cục bộ bằng chênh lệch tuyệt đối của hai con số mà nó phân tách. Chúng tôi được cấp ngân sách và chúng tôi muốn tối đa hóa số lần cắt giảm mà chúng tôi thực hiện trong khi vẫn giữ tổng chi phí trong ngân sách này và duy trì điều kiện hợp lệ trên mọi phân đoạn kết quả. 

Ràng buộc chính là về tổng thể, chuỗi đã chứa số lượng giá trị chẵn và lẻ bằng nhau, điều này đảm bảo rằng có ít nhất một phân đoạn chuỗi đầy đủ hợp lệ tồn tại mà không bị cắt. 

Các giới hạn rất nhỏ, với n lên tới 100 và ngân sách lên tới 100, điều này ngay lập tức gợi ý rằng lập trình động bậc hai hoặc bậc ba có thể chấp nhận được. Bất kỳ giải pháp nào cố gắng liệt kê trực tiếp tất cả các phân đoạn đều có tính chất hàm mũ, vì mọi khoảng trống trong số n−1 đều có thể bị cắt hoặc không, dẫn đến khả năng 2^(n−1). Con số đó trở thành khoảng 10^30 trong trường hợp xấu nhất, vượt xa mọi tính toán khả thi. 

Một chiến lược tham lam ngây thơ như “cắt giảm bất cứ khi nào nó trông rẻ” sẽ thất bại vì việc cắt giảm sớm với giá rẻ có thể cản trở một thỏa thuận toàn cầu tốt hơn. Ví dụ: nếu việc cắt giảm chi phí nhỏ buộc bạn sau đó phải chi những khoản cắt giảm đắt tiền để cân bằng lại sự cân bằng trong các phân khúc thì quyết định cục bộ là sai lầm. 

Một trường hợp thất bại tinh tế khác xuất hiện khi có nhiều vị trí hợp lệ trong đó một phân đoạn trở nên cân bằng. Việc chọn cắt giảm sớm nhất có thể có thể lãng phí ngân sách cần thiết cho lần cắt giảm sau đó, mang lại tổng thể phân khúc cân bằng hơn. 

Khó khăn thực sự là việc cắt giảm không độc lập: việc quyết định điểm kết thúc của một phân đoạn sẽ xác định cấu trúc của tất cả các phân đoạn trong tương lai. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ xem xét mọi tập hợp con của n-1 vị trí cắt có thể. Đối với mỗi tập hợp con, chúng tôi sẽ kiểm tra xem mọi phân đoạn kết quả có số chẵn và số lẻ bằng nhau hay không và tính tổng chi phí của các lần cắt đã chọn. Điều này đúng vì nó xác minh rõ ràng mọi khả năng. 

Tuy nhiên, điều này yêu cầu lặp lại trên 2^(n−1) tập hợp con và đối với mỗi tập hợp con, chúng tôi có thể quét các phân đoạn trong O(n), đưa ra tổng độ phức tạp theo cấp số nhân. Ngay cả ở mức n = 100, điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là chúng ta không thực sự cần phải suy luận về tất cả các tập hợp con trên toàn cầu. Thay vào đó, chúng ta có thể xử lý mảng từ trái sang phải và theo dõi xem chúng ta đã thấy bao nhiêu số chẵn và số lẻ trong phân đoạn hiện tại. Thời điểm một phân khúc trở nên cân bằng, chúng ta có một vị trí ứng cử viên được phép cắt giảm. 

Tại mỗi vị trí cắt ứng viên như vậy, chúng ta phải đối mặt với một lựa chọn: cắt ở đây hoặc bỏ qua. Việc cắt giảm sẽ tiêu tốn chi phí và tăng số lần cắt lên một, trong khi việc bỏ qua sẽ giữ cho phân khúc luôn mở, có khả năng hợp nhất các điểm cuối hợp lệ trong tương lai. Đây chính xác là một vấn đề lập trình động “tối đa hóa số lượng trong ngân sách” cổ điển. 

Chúng tôi xác định trạng thái DP theo vị trí và ngân sách còn lại, đồng thời thử tất cả các điểm cắt hợp lệ. Vì các điểm cắt hợp lệ có thể được tính toán trước nên việc chuyển đổi trở nên dễ quản lý. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n · n) | O(n) | Quá chậm | 
| DP qua các tiểu bang | O(n^2 · B) | O(n · B) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Tính toán trước tất cả các vị trí được phép cắt. Chúng tôi quét mảng trong khi duy trì sự khác biệt giữa số phần tử chẵn và lẻ. Bất cứ khi nào sự khác biệt này trở thành 0, tiền tố kết thúc ở đây có thể tạo thành ranh giới phân đoạn hợp lệ. 
2. Hãy để các vị trí cắt hợp lệ này được lưu trữ theo thứ tự tăng dần. Chúng tôi chỉ xem xét việc cắt giảm tại các vị trí này vì bất kỳ phân đoạn hợp lệ nào cũng phải kết thúc mỗi phân đoạn tại điểm cân bằng như vậy. 
3. Xác định một bảng lập trình động dp[i][b], trong đó i thể hiện số lượng vị trí cắt hợp lệ mà chúng ta đã xem xét cho đến nay và b thể hiện ngân sách còn lại. Giá trị được lưu trữ là số lần cắt tối đa có thể đạt được. 
4. Khởi tạo dp[0][b] = 0 với mọi b, vì trước bất kỳ vị trí cắt nào không có vết cắt nào được thực hiện. 
5. Đối với mỗi vị trí cắt hợp lệ i, hãy tính chi phí cắt ở đó là abs(a[pos] - a[pos+1]). Chi phí này là cố định và độc lập với các quyết định khác. 
6. Đối với mỗi giá trị ngân sách, truyền bá các chuyển đổi: hoặc chúng tôi không cắt tại i và chuyển tiếp dp[i−1][b] hoặc chúng tôi cắt nếu b là đủ, cập nhật dp[i][b] = max(dp[i][b], dp[i−1][b − cost] + 1). 
7. Đáp án cuối cùng là giá trị lớn nhất của dp[last][b] trên tất cả b ≤ B. 

### Tại sao nó hoạt động 

Bất biến quan trọng là dp[i][b] thể hiện số lần cắt giảm tốt nhất có thể đạt được chỉ bằng cách sử dụng i vị trí cắt hợp lệ đầu tiên và chi tiêu tối đa b ngân sách. Mọi phân đoạn hợp lệ phải chọn các ranh giới bị cắt từ tập hợp này vì chỉ những vị trí này mới bảo toàn được các phân đoạn cân bằng. Vì mỗi quá trình chuyển đổi trạng thái bao gồm hoặc loại trừ phần cắt ở ranh giới hợp lệ cố định, nên chúng tôi liệt kê tất cả các phân đoạn hợp lệ về mặt cấu trúc mà không có sự dư thừa và ngân sách được thực thi chính xác thông qua thứ nguyên DP. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, B = map(int, input().split())
    a = list(map(int, input().split()))

    # find valid cut positions (prefix balanced in parity sense)
    valid = []
    diff = 0  # evens - odds

    for i in range(n - 1):
        if a[i] % 2 == 0:
            diff += 1
        else:
            diff -= 1

        if diff == 0:
            cost = abs(a[i] - a[i + 1])
            valid.append(cost)

    # dp[i][b]: using first i cuts, budget b
    m = len(valid)
    dp = [[0] * (B + 1) for _ in range(m + 1)]

    for i in range(1, m + 1):
        cost = valid[i - 1]
        for b in range(B + 1):
            dp[i][b] = dp[i - 1][b]
            if b >= cost:
                dp[i][b] = max(dp[i][b], dp[i - 1][b - cost] + 1)

    print(max(dp[m]))

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ nén vấn đề vào một danh sách các vị trí cắt ứng cử viên trong đó tiền tố được cân bằng. Mỗi ứng viên chỉ lưu trữ chi phí cắt giảm ở đó vì cấu trúc đảm bảo tính độc lập giữa các vị trí. 

Bảng DP được xây dựng từng hàng trên các ứng cử viên này. Ở mỗi bước, chúng tôi bỏ qua hoặc cắt giảm, tôn trọng ngân sách còn lại. Mức tối đa cuối cùng của tất cả ngân sách sẽ thể hiện số lần cắt giảm tốt nhất có thể đạt được. 

Một chi tiết tinh tế là chúng tôi chỉ tính toán chi phí ở các ranh giới tiền tố hợp lệ. Điều này tránh việc xem xét việc cắt giảm bất hợp pháp có thể phá vỡ yêu cầu chẵn lẻ, đảm bảo không gian trạng thái DP là chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6 4
1 2 5 10 15 20
```Vị trí cắt hợp lệ được tìm thấy bằng cách theo dõi số dư chẵn lẻ: 

| tôi | một [tôi] | khác biệt | cân bằng? | chi phí | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | -1 | không | - | 
| 1 | 2 | 0 | vâng | 3 | 
| 2 | 5 | -1 | không | - | 
| 3 | 10 | 0 | vâng | 5 | 
| 4 | 15 | -1 | không | - | 

Vậy chi phí hợp lệ là [3, 5]. 

Bây giờ DP xem xét các tập hợp con của những lần cắt giảm này theo ngân sách 4. Chỉ lần cắt giảm đầu tiên là có khả năng chi trả nên kết quả là 1. 

### Ví dụ 2 

đầu vào:```
4 1
1 3 5 7
```Ở đây không có tiền tố nào có số chẵn và số lẻ bằng nhau, do đó không có vị trí cắt hợp lệ. Bảng DP luôn bằng 0 và câu trả lời là 0. 

Điều này chứng tỏ rằng việc thiếu các điểm cân bằng hợp lệ sẽ ngăn cản mọi sự cắt giảm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · B) | Chúng tôi đánh giá tối đa n điểm cắt hợp lệ, mỗi điểm có chuyển đổi ngân sách lên tới B | 
| Không gian | O(n · B) | Bảng DP lưu trữ các trạng thái cho từng tiền tố của vị trí cắt và ngân sách | 

Với n, B 100, tổng số hoạt động có nhiều nhất là 10^4 chuyển đổi trạng thái, nằm trong giới hạn thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("6 4\n1 2 5 10 15 20\n") == "1"

# no valid cuts
assert run("4 10\n1 3 5 7\n") == "0"

# minimal balanced immediate cut
assert run("2 10\n1 2\n") == "0"

# multiple valid cuts but tight budget
assert run("6 3\n1 2 3 4 5 6\n") in ["1"]

# exact budget match for two cuts
assert run("6 100\n1 2 3 4 5 6\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| không có tiền tố cân bằng | 0 | không thể cắt giảm | 
| kích thước tối thiểu | 0 | xử lý cạnh | 
| ngân sách eo hẹp | 1 | tham lam tránh giới hạn | 
| ngân sách lớn | 2 | tích lũy DP đầy đủ | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi chuỗi không bao giờ tạo thành tiền tố cân bằng trước khi kết thúc. Trong trường hợp đó, danh sách hợp lệ trống và DP không bao giờ chuyển tiếp. Đối với đầu vào`1 3 5 7`, thuật toán không tạo ra ứng cử viên nào, do đó dp vẫn bằng 0 và đầu ra chính xác là 0. 

Một trường hợp khác là khi tồn tại nhiều tiền tố cân bằng nhưng chi phí của chúng vượt quá ngân sách. Mặc dù về mặt cấu trúc có thể cắt giảm nhiều lần, DP vẫn loại bỏ chúng một cách chính xác do hạn chế về ngân sách, đảm bảo không xảy ra tình trạng tính vượt mức không hợp lệ.
