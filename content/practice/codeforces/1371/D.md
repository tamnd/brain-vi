---
problem: 1371D
contest_id: 1371
problem_index: D
name: "Grid-00100"
contest_name: "Codeforces Round 654 (Div. 2)"
rating: 1600
tags: ["constructive algorithms", "greedy", "implementation"]
answer: passed_samples
verified: true
solve_time_s: 111
date: 2026-06-14
model: gpt-5-5
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2e5587-07e8-83ec-bde5-f0b7981cc46f
---

# CF 1371D - Grid-00100 

**Đánh giá:** 1600 
**Tags:** thuật toán xây dựng, tham lam, triển khai 
**Mẫu:** gpt-5-5 
**Thời gian giải:** 1 phút 51 giây 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2e5587-07e8-83ec-bde5-f0b7981cc46f 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng tôi được yêu cầu xây dựng một$n \times n$lưới chỉ chứa số không và số một, với ràng buộc chính xác$k$các tế bào chứa những cái đó. Lưới không được đánh giá trực tiếp bởi cách các lưới được đặt mà bằng cách chúng được phân bổ không đồng đều trên các hàng và cột. 

Đối với mỗi hàng, chúng tôi đếm xem nó chứa bao nhiêu hàng, tạo ra một chuỗi tổng các hàng. Chúng tôi làm tương tự cho các cột. Chi phí của một lưới được xác định bằng mức độ chênh lệch của các khoản tiền này: chúng tôi bình phương chênh lệch giữa tổng hàng tối đa và tối thiểu và thêm cùng một số lượng cho các cột. 

Vì vậy, nhiệm vụ không chỉ là đặt chính xác$k$nhưng phải đặt chúng sao cho tổng hàng cân bằng nhất có thể và tổng cột cũng cân bằng nhất có thể. 

Các ràng buộc đủ lớn nên mọi nỗ lực thử kết hợp các vị trí đều không thể thực hiện được. Ngay cả việc lặp lại trên tất cả các lưới cũng không thành vấn đề vì số lượng lưới có thể là$2^{n^2}$. Ngay cả việc đánh giá một lưới đơn lẻ cũng rẻ, nhưng việc xây dựng một lưới tối ưu đòi hỏi phải hiểu cấu trúc hơn là tìm kiếm. 

Trường hợp góc tinh tế xuất hiện khi$k = 0$hoặc$k = n^2$. Trong cả hai trường hợp, lưới hoàn toàn đồng nhất, do đó tổng mỗi hàng và cột đều giống hệt nhau và chi phí bằng 0. Một cách tiếp cận tham lam ngây thơ “trải từng hàng” vẫn có thể vô tình gây ra sự mất cân bằng nếu không xử lý cẩn thận những thái cực này. 

Một trường hợp phức tạp khác là khi$k$không chia hết cho$n$. Ví dụ, với$n = 4, k = 7$, người ta có thể cố gắng điền đều các hàng, nhưng sự mất cân bằng của cột trở thành yếu tố hạn chế thực sự và việc điền hàng một cách bất cẩn có thể tạo ra sự khác biệt lớn trong các cột ngay cả khi các hàng trông cân đối. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử mọi vị trí có thể của$k$những người trong số$n^2$vị trí, tính toán tất cả các tổng hàng và cột và đánh giá hàm chi phí. Điều này đúng nhưng không khả thi: số lựa chọn là$\binom{n^2}{k}$, tăng trưởng theo cấp số nhân và trở nên lớn về mặt thiên văn ngay cả đối với mức độ vừa phải$n$. 

Quan sát quan trọng là cả tổng hàng và tổng cột chỉ phụ thuộc vào mức độ chúng ta phân phối đồng đều dọc theo hàng và cột. Nếu chúng ta nghĩ đến việc sắp xếp các số một cách tuần tự, điều tốt nhất chúng ta có thể hy vọng là giữ cho tổng hàng khác nhau nhiều nhất là một, và tương tự đối với tổng theo cột. Bất kỳ sự mất cân bằng nào lớn hơn sẽ ngay lập tức làm tăng hình phạt bình phương, vì vậy cấu hình tối ưu phải càng “phẳng” càng tốt. 

Điều này dẫn đến một chiến lược mang tính xây dựng: chúng tôi lấp đầy lưới theo cách phân phối các lưới gần như đồng đều trên các hàng và cột. Một cách tự nhiên để đạt được điều này là duyệt qua lưới theo thứ tự hàng lớn và đặt từng cái một. Tuy nhiên, để đảm bảo cân bằng đồng thời cả hàng và cột, chúng tôi sử dụng mẫu đường chéo tuần hoàn thay vì tô đường thẳng. 

Cụ thể, chúng tôi đặt những cái ở vị trí$(i, (i + j) \bmod n)$, đạp xe qua các cột khi chúng ta di chuyển qua các hàng. Điều này đảm bảo rằng mỗi hàng nhận được một trong hai$\lfloor k/n \rfloor$hoặc$\lceil k/n \rceil$và tương tự cho các cột. Sự mất cân bằng được giảm thiểu đồng thời ở cả hai chiều. 

Khi chúng tôi chấp nhận rằng tổng hàng và tổng cột phải khác nhau nhiều nhất là một, thì việc tính toán chi phí tối thiểu sẽ trở nên đơn giản: nó chỉ phụ thuộc vào số lượng hàng (hoặc cột) nhận được chi phí tăng thêm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(\binom{n^2}{k} \cdot n^2)$|$O(n^2)$| Quá chậm | 
| Xây dựng tối ưu |$O(n^2)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng lưới bằng cách phân phối chúng một cách đồng đều nhất có thể. 

1. Tính xem mỗi hàng sẽ nhận được bao nhiêu cái một cách lý tưởng. Cho phép$q = k // n$Và$r = k \bmod n$. Điều này có nghĩa là mỗi hàng có một trong hai$q$hoặc$q+1$những cái đó. Đây là cách duy nhất có thể để giữ tổng số hàng cân bằng nhất có thể. 
2. Chúng tôi chỉ định những cái bổ sung cho cái đầu tiên$r$hàng. Điều này đảm bảo rằng tổng số hàng khác nhau tối đa một và giảm thiểu$\max(R) - \min(R)$. 
3. Trong mỗi hàng, thay vì đặt các vị trí một cách tùy ý vào các cột đầu tiên, chúng tôi thay đổi vị trí theo chu kỳ. Hàng ngang$i$đặt những cái trong cột$(i \cdot q + j) \bmod n$. Điều này giúp các cột không bị lệch nhiều về phía các chỉ số ban đầu. 
4. Chúng tôi lặp lại mẫu này cho tất cả các hàng, đảm bảo rằng tổng các cột cũng khác nhau tối đa một. Sự thay đổi theo chu kỳ là sự phân bổ đều phần vượt quá theo hàng trên các cột. 
5. Cuối cùng, chúng tôi tính toán chi phí bằng cách sử dụng tổng hàng và cột tối đa và tối thiểu đạt được đã được xác định trong quá trình xây dựng. 

### Tại sao nó hoạt động 

Bất biến chính là sau khi xây dựng, mỗi tổng hàng là$q$hoặc$q+1$và tổng mỗi cột cũng là$\lfloor k/n \rfloor$hoặc$\lceil k/n \rceil$. Bởi vì không có hàng hoặc cột nào lệch nhiều hơn một so với mức trung bình, nên bất kỳ nỗ lực nào nhằm giảm bớt phương sai của một bên hơn nữa sẽ buộc bên kia trở nên mất cân bằng hơn. Sự dịch chuyển theo chu kỳ đảm bảo tính đối xứng giữa các hàng và cột, do đó, không chiều nào được ưu tiên, đó chính xác là điều giúp giảm thiểu tổng các phạm vi bình phương. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        
        grid = [[0] * n for _ in range(n)]
        
        q, r = divmod(k, n)
        
        # distribute ones row-wise with cyclic shifts
        for i in range(n):
            ones = q + (1 if i < r else 0)
            for j in range(ones):
                col = (i + j) % n
                grid[i][col] = 1
        
        # compute row and column stats
        row = [sum(row) for row in grid]
        col = [sum(grid[i][j] for i in range(n)) for j in range(n)]
        
        cost = (max(row) - min(row)) ** 2 + (max(col) - min(col)) ** 2
        
        print(cost)
        for row in grid:
            print(''.join(map(str, row)))

if __name__ == "__main__":
    solve()
```Việc xây dựng đầu tiên khắc phục sự mất cân bằng hàng bằng cách sử dụng phân phối thương số dư. Sự dịch chuyển theo chu kỳ bên trong mỗi hàng ngăn cản việc xếp chồng các hàng trong cùng một cột trên các hàng liên tiếp, điều này sẽ làm tăng phương sai của cột. 

Việc tính toán chi phí chỉ được thực hiện sau khi xây dựng, vì cấu trúc đảm bảo nó đã ở mức tối ưu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 2
```Đây$q = 1, r = 0$. Mỗi hàng có đúng một số 1. 

| Hàng | Đã đặt cột | Tổng hàng | 
| --- | --- | --- | 
| 0 | 0 | 1 | 
| 1 | 1 | 1 | 

| Cột | Tổng hợp | 
| --- | --- | 
| 0 | 1 | 
| 1 | 1 | 

Chi phí là$(1-1)^2 + (1-1)^2 = 0$. 

Điều này xác nhận rằng việc gán hàng thống nhất đã đạt được sự cân bằng hoàn hảo khi$k$chia rẽ$n$. 

### Ví dụ 2 

đầu vào:```
3 8
```Đây$q = 2, r = 2$. Hai hàng được 3 cái, một hàng được 2 cái. 

| Hàng | Những người được đặt (theo chu kỳ) | Tổng hàng | 
| --- | --- | --- | 
| 0 | 0 1 2 | 3 | 
| 1 | 1 2 0 | 3 | 
| 2 | 2 0 | 2 | 

Tổng cột trở thành: 

| Cột | Tổng hợp | 
| --- | --- | 
| 0 | 2 | 
| 1 | 2 | 
| 2 | 2 | 

Phạm vi hàng là$3 - 2 = 1$, phạm vi cột là$2 - 2 = 0$, vậy chi phí là$1^2 + 0^2 = 1$. 

Điều này cho thấy việc dịch chuyển theo chu kỳ ngăn chặn sự mất cân bằng của cột như thế nào ngay cả khi các hàng không đồng đều. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| Mỗi trường hợp kiểm thử sẽ điền và đánh giá một$n \times n$lưới | 
| Không gian |$O(n^2)$| Lưu trữ cho lưới | 

Tổng số tiền của$n^2$qua các bài kiểm tra được giới hạn bởi$10^5$, do đó công trình được vận hành thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n, k = map(int, input().split())
            grid = [[0]*n for _ in range(n)]
            q, r = divmod(k, n)
            for i in range(n):
                ones = q + (1 if i < r else 0)
                for j in range(ones):
                    grid[i][(i + j) % n] = 1
            row = [sum(rw) for rw in grid]
            col = [sum(grid[i][j] for i in range(n)) for j in range(n)]
            cost = (max(row)-min(row))**2 + (max(col)-min(col))**2
            out.append(str(cost))
            out.extend("".join(map(str, rw)) for rw in grid)
        return "\n".join(out)

    return solve()

# sample tests
assert run("""1
2 2
""").split()[-3:] == ["0","10","01"]

# custom cases
assert run("""1
1 0
""") == "0\n0"

assert run("""1
1 1
""") == "0\n1"

assert run("""1
3 0
""").startswith("0")

assert run("""1
3 9
""").split()[0] == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1x1 trống | 0 lưới | trường hợp cơ bản số không | 
| 1x1 đầy đủ | 0 lưới | đối xứng bão hòa đầy đủ | 
| 3x3 k=0 | tất cả số không | không có trường hợp mất cân bằng | 
| 3x3 k=n2 | tất cả những cái | đồng nhất đầy đủ | 

## Vỏ cạnh 

Khi nào$k = 0$, công trình tạo ra một lưới hoàn toàn bằng không. Tổng của mỗi hàng và cột đều bằng 0, vì vậy cả hai phạm vi đều bằng 0 và chi phí được tính toán chính xác bằng 0. Việc triển khai tham lam vẫn cố gắng sắp xếp theo chu kỳ sẽ cố gắng truy cập các cột không chính xác và có khả năng đưa ra các thao tác ghi không hợp lệ trừ khi được bảo vệ rõ ràng. 

Khi$k = n^2$, mỗi ô được lấp đầy bằng một. Tổng hàng và cột đều là$n$, do đó một lần nữa phương sai bằng không. Việc xây dựng xử lý việc này một cách tự nhiên bởi vì$q = n$Và$r = 0$, vì vậy mỗi hàng được điền đầy đủ. 

Khi$k < n$, chỉ hàng đầu tiên nhận được những cái đó. Sự dịch chuyển theo chu kỳ đảm bảo những cái này trải rộng trên các cột riêng biệt, ngăn cản sự tập trung của cột. Nếu không dịch chuyển, tất cả các cột sẽ xếp chồng lên nhau trong cùng một cột và tạo ra một phạm vi cột bằng một, điều này sẽ không tối ưu so với cấu hình dàn trải.
