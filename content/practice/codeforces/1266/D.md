---
problem: 1266D
contest_id: 1266
problem_index: D
name: "Decreasing Debts"
contest_name: "Codeforces Global Round 6"
rating: 2000
tags: ["constructive algorithms", "data structures", "graphs", "greedy", "implementation", "math", "two pointers"]
answer: passed_samples
verified: true
solve_time_s: 197
date: 2026-06-13
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2d8730-d1b0-83ec-b40b-a8447d19d422
---

#CF 1266D - Giảm Nợ 

**Xếp hạng:** 2000 
**Tags:** thuật toán xây dựng, cấu trúc dữ liệu, đồ thị, tham lam, triển khai, toán học, hai con trỏ 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 3 phút 17s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2d8730-d1b0-83ec-b40b-a8447d19d422 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta được cho một đồ thị có trọng số có hướng trong đó mỗi đỉnh đại diện cho một người và mỗi cạnh có hướng đại diện cho một món nợ từ người này sang người khác. Giá trị của một lợi thế là số tiền mà người nguồn nợ người đích. Nhiệm vụ không phải là giảm thiểu số lượng các khoản nợ mà là chuyển đổi cấu trúc của các khoản nợ này bằng cách sử dụng hai phép toán được phép sao cho tổng của tất cả các trọng số cạnh trở nên nhỏ nhất có thể, trong khi vẫn giữ được biểu diễn hợp lệ. 

Hoạt động đầu tiên là định tuyến lại hai khoản nợ độc lập: nếu một người nợ người khác và tồn tại một khoản nợ không liên quan thứ hai, chúng ta có thể phân phối lại một số tiền giữa các kết nối chéo. Điều này cho phép chuyển dòng nợ một cách hiệu quả thông qua cơ cấu trung gian. Hoạt động thứ hai sẽ xóa nợ bản thân, nghĩa là bất kỳ chu kỳ nào rơi vào tình trạng ai đó mắc nợ bản thân đều có thể bị xóa hoàn toàn. 

Giải thích chính là các khoản nợ có thể được "định tuyến lại" thông qua các nút trung gian và các vòng lặp tự có thể được loại bỏ miễn phí. Mục tiêu là tìm ra một đại diện tương đương cho cùng một sự mất cân đối tài chính ròng nhưng có tổng trọng số biên định hướng tối thiểu. 

Các ràng buộc rất lớn, lên tới 100.000 người và 300.000 khoản nợ, mỗi khoản lên tới 10^9. Điều này ngay lập tức loại trừ mọi cách xây dựng bậc hai hoặc việc hợp nhất các cạnh theo cặp lặp đi lặp lại. Bất kỳ giải pháp nào cũng phải gần tuyến tính về số cạnh, có thể là O(n + m) hoặc O(m log m). Bộ nhớ cũng đủ chặt chẽ để chúng ta phải tránh xây dựng các ma trận kề cận dày đặc hoặc các phép mở rộng lặp đi lặp lại của đồ thị. 

Một vấn đề tế nhị phát sinh khi các chu kỳ tồn tại. Một cách tiếp cận đơn giản có thể cố gắng hủy bỏ một cách tham lam các luồng đối diện hoặc hợp nhất các đường dẫn cục bộ, nhưng điều này không thành công khi nhiều chu trình tương tác với nhau. Một trường hợp phức tạp khác là khi một đỉnh là một phần của nhiều khoản nợ đến và đi; việc hủy bỏ theo cặp ngây thơ có thể để lại các chu kỳ dư không tối ưu mà vẫn đóng góp tổng trọng số không cần thiết. 

Ví dụ: nếu 1→2 có 10, 2→3 có 10 và 3→1 có 10, việc hủy đơn giản có thể không nhận ra rằng toàn bộ chu trình có thể được định tuyến lại và loại bỏ, chỉ để lại chi phí ròng bằng 0 sau khi loại bỏ vòng lặp tự. 

Thách thức cốt lõi là nhận ra rằng chỉ có sự mất cân bằng ròng ở mỗi nút mới quan trọng chứ không phải việc định tuyến chính xác các luồng. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực sẽ cố gắng áp dụng nhiều lần hai thao tác cho đến khi không thể cải thiện được. Điều này sẽ liên quan đến việc chọn các cặp cạnh, mô phỏng các tuyến đường lại và theo dõi các vòng tự lặp, có khả năng khám phá các kết hợp O(m^2). Mỗi thao tác có thể thay đổi nhiều cạnh nên việc hội tụ có thể mất nhiều bước. Điều này là quá chậm khi tôi lên tới 300.000. 

Điểm mấu chốt là các hoạt động cho phép hoàn toàn tự do phân phối lại các luồng, miễn là các hạn chế về luồng vào và luồng ra được duy trì. Điều này có nghĩa là vấn đề giảm xuống còn việc theo dõi mỗi nút là con nợ hoặc chủ nợ ròng bao nhiêu. 

Nếu chúng tôi tính toán số dư ròng cho mỗi nút, được định nghĩa là số tiền gửi đi trừ đi số dư đến, thì các nút dương phải gửi tiền và các nút âm phải nhận tiền. Cấu trúc của các cạnh trung gian không liên quan vì hoạt động loại 1 cho phép định tuyến lại luồng tùy ý giữa các cặp cạnh. Điều này có nghĩa là chúng ta có thể nén biểu đồ thành một bài toán chuyển giao hai bên giữa các nút thặng dư và thiếu hụt. 

Khi các nút được phân loại theo số dư ròng, chúng ta chỉ cần kết nối các nút dư thừa với các nút thiếu hụt một cách tham lam. Các vòng lặp tự động biến mất vì chúng thể hiện sự hủy bỏ nội bộ của số dư của nút. 

Chúng tôi sắp xếp hoặc thu thập tất cả số dư dương và số dư âm, sau đó đối chiếu chúng bằng chiến lược hai con trỏ. Điều này xây dựng một biểu diễn tối thiểu hợp lệ với tối đa n-1 cạnh.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(m^2) | O(m) | Quá chậm | 
| Số dư ròng + Đối sánh tham lam | O(m + n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán số dư ròng cho mỗi nút bằng cách cộng nợ đi và trừ đi nợ đến. Điều này nắm bắt xem một nút nói chung là chủ nợ hay con nợ sau khi tất cả các lần hủy cục bộ được áp dụng về mặt khái niệm. 
2. Chia các nút thành hai danh sách: danh sách có số dư dương và danh sách có số dư âm. Các nút dương đại diện cho những người nên trả tiền, các nút âm đại diện cho những người nên nhận tiền. 
3. Sử dụng hai con trỏ trên các danh sách này. Ở mỗi bước, khớp nút dương hiện tại với nút âm hiện tại. Số tiền được chuyển là số tiền tối thiểu trong số dư tuyệt đối của họ. Điều này đảm bảo một trong số họ trở nên trung lập sau khi chuyển giao. 
4. Ghi lại lần chuyển này làm cạnh cuối cùng ở đầu ra. Giảm cả hai số dư cho phù hợp. 
5. Di chuyển con trỏ khi số dư của nút trở về 0. Tiếp tục cho đến khi hết số dư. 
6. Xuất ra tất cả các cạnh được xây dựng. Chúng đại diện cho một hệ thống nợ hợp nhất hợp lệ với tổng số tiền tối thiểu. 

Lý do hoạt động ghép nối tham lam này là vì bất kỳ chuỗi hoạt động được phép hợp lệ nào cũng chỉ duy trì số dư ròng ở mỗi nút. Do đó, cấu hình cuối cùng hoàn toàn được xác định bởi các số dư này và mọi sự phân phối lại giữa các cặp đều có thể hoán đổi cho nhau. 

### Tại sao nó hoạt động 

Các hoạt động được phép bảo toàn luồng mạng ở mỗi đỉnh. Thao tác một chỉ định tuyến lại luồng mà không thay đổi tổng số vào hoặc ra trên mỗi nút và thao tác hai loại bỏ luồng tự nội bộ mà không ảnh hưởng đến bất kỳ nút nào khác. Điều này có nghĩa là mọi nút đều có một giá trị bất biến: số dư ròng của nó. 

Bất kỳ cấu hình cuối cùng hợp lệ nào cũng phải đáp ứng rằng số dư đầu ra trừ đi đầu vào của mỗi nút bằng số dư tính toán ban đầu của nút đó. Phương pháp khớp tham lam xây dựng một tập hợp cạnh tối thiểu thỏa mãn chính xác các ràng buộc này mà không tạo ra sự lưu thông trung gian bổ sung. Vì mỗi đơn vị dòng chảy được kết hợp trực tiếp giữa thặng dư và thâm hụt, nên không có chu trình dư thừa nào được hình thành và không có vòng tự lặp nào xuất hiện trong cấu trúc cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    balance = [0] * (n + 1)

    for _ in range(m):
        u, v, d = map(int, input().split())
        balance[u] += d
        balance[v] -= d

    pos = []
    neg = []

    for i in range(1, n + 1):
        if balance[i] > 0:
            pos.append([i, balance[i]])
        elif balance[i] < 0:
            neg.append([i, -balance[i]])

    i = j = 0
    res = []

    while i < len(pos) and j < len(neg):
        u, a = pos[i]
        v, b = neg[j]

        x = min(a, b)
        res.append((u, v, x))

        pos[i][1] -= x
        neg[j][1] -= x

        if pos[i][1] == 0:
            i += 1
        if neg[j][1] == 0:
            j += 1

    print(len(res))
    for u, v, d in res:
        print(u, v, d)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách nén tất cả các khoản nợ vào một mảng số dư duy nhất. Mỗi cạnh đóng góp tích cực cho con nợ và tiêu cực cho chủ nợ. Điều này loại bỏ tất cả sự phức tạp về cấu trúc của biểu đồ. 

Sau đó chúng tôi tách các nút bằng dấu hiệu cân bằng. Vòng lặp hai con trỏ là giai đoạn xây dựng trong đó chúng ta xây dựng tập hợp các cạnh cuối cùng một cách rõ ràng. Mỗi bước đảm bảo tiến độ vì ít nhất một nút được giải quyết hoàn toàn. 

Đầu ra cuối cùng trực tiếp mã hóa các chuyển đổi cần thiết tối thiểu mà không cần xây dựng các chu trình trung gian hoặc vòng tự lặp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
1 2 10
2 3 5
```Số dư: 

| Nút | Tính toán | Số dư | 
| --- | --- | --- | 
| 1 | +10 | 10 | 
| 2 | -10 + 5 | -5 | 
| 3 | -5 | -5 | 

Chúng ta tạo thành pos = [(1,10)] và neg = [(2,5), (3,5)]. 

Bảng bước: 

| tôi | j | tư thế | phủ định | chuyển nhượng | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 10 | 5 | 5 | 
| 1 | 2 | 5 | 5 | 5 | 

Các cạnh cuối cùng: 

1→2 (5), 1→3 (5) 

Điều này cho thấy nút trung gian 2 không có cấu trúc đặc biệt trong cấu trúc cuối cùng; chỉ có dòng chảy ròng là quan trọng. 

### Ví dụ 2 

đầu vào:```
4 3
1 2 8
2 3 3
3 1 5
```Số dư: 

| Nút | Số dư | 
| --- | --- | 
| 1 | 8 - 5 = 3 | 
| 2 | 3 - 8 = -5 | 
| 3 | 5 - 3 = 2 | 
| 4 | 0 | 

pos = [(1,3), (3,2)], neg = [(2,5)]. 

Các bước: 

| tôi | j | tư thế | phủ định | chuyển nhượng | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 3 | 5 | 3 | 
| 2 | 1 | 2 | 2 | 2 | 

Các cạnh cuối cùng: 

1→2 (3), 3→2 (2) 

Điều này xác nhận rằng các chu kỳ được hấp thụ hoàn toàn vào số dư ròng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Một lượt để tính toán số dư và một lượt đối sánh tuyến tính | 
| Không gian | O(n) | Lưu trữ mảng cân bằng và danh sách cạnh cuối cùng | 

Thuật toán chia tỷ lệ trực tiếp theo số cạnh, phù hợp thoải mái trong giới hạn lên tới 300.000 thao tác. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdout = old_stdout

# sample 1
assert run("""3 2
1 2 10
2 3 5
""") == """2
1 2 5
1 3 5"""

# single edge
assert run("""2 1
1 2 7
""") == """1
1 2 7"""

# already balanced cycle
assert run("""3 3
1 2 5
2 3 5
3 1 5
""") == """2
1 2 5
3 2 5"""

# all zero after cancellation
assert run("""3 2
1 2 10
2 1 10
""") == """0"""

# star structure
assert run("""4 3
1 2 4
1 3 6
1 4 2
""") == """3
1 2 4
1 3 6
1 4 2"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hủy bỏ chu kỳ | 2 cạnh | nén chu trình về dạng cân bằng | 
| cạnh đơn | 1 cạnh | độ đúng cơ sở | 
| hủy bỏ hai chiều | 0 | xử lý hủy bỏ đầy đủ | 
| ngôi sao | các cạnh giống nhau | phân phối thặng dư thuần túy | 

## Vỏ cạnh 

Trường hợp hủy hai nút thuần túy như 1→2 với 10 và 2→1 với 10 kiểm tra xem thuật toán có tạo ra đầu ra bằng 0 một cách chính xác hay không. Tính toán số dư cho cả hai nút bằng 0, vì vậy cả hai danh sách đều trống và không có cạnh nào được phát ra, phù hợp với việc loại bỏ tất cả nợ dự kiến. 

Một chu kỳ dài như 1→2→3→…→k→1 kiểm tra xem liệu tuần hoàn bên trong có tồn tại không chính xác hay không. Mảng số dư trở thành toàn số 0 và thuật toán không đưa ra kết quả gì, xác nhận rằng các chu kỳ không đóng góp vào khoản nợ cuối cùng. 

Một ngôi sao bị lệch trong đó một nút có các cạnh đi ra lớn và nhiều cạnh đi vào nhỏ kiểm tra độ ổn định của con trỏ. Kết hợp tham lam đảm bảo rằng việc chuyển một phần được phân chia chính xác mà không tạo thêm các nút trung gian, duy trì độ phức tạp và tính chính xác tuyến tính.
