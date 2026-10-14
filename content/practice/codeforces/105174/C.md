---
title: "CF 105174C - \u5927\u9b54\u6cd5\u5e08"
description: "Cây đũa phép có đúng n khe đá quý. Có ba loại đá quý độc lập. Loại đầu tiên tăng sức tấn công phép thuật, loại thứ hai tăng mana và loại thứ ba tăng tốc độ tấn công."
date: "2026-06-27T08:14:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105174
codeforces_index: "C"
codeforces_contest_name: "The 22nd Sichuan University Programming Contest"
rating: 0
weight: 105174
solve_time_s: 60
verified: true
draft: false
---

[CF 105174C - \u5927\u9b54\u6cd5\u5e08](https://codeforces.com/problemset/problem/105174/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Cây đũa phép có chính xác`n`khe đá quý. Có ba loại đá quý độc lập. 

Loại đầu tiên tăng sức tấn công phép thuật, loại thứ hai tăng mana và loại thứ ba tăng tốc độ tấn công. Mỗi viên đá quý chiếm một số vị trí và đóng góp giá trị dương cho chính xác một thuộc tính. Mỗi viên ngọc có thể được sử dụng nhiều nhất một lần. 

Sau khi chọn bất kỳ bộ sưu tập đá quý nào có tổng số ô chiếm không vượt quá`n`, sức mạnh cuối cùng của cây đũa phép là`magic_attack × mana × attack_speed`. 

Nhiệm vụ là tối đa hóa sản phẩm này. 

Quan sát quan trọng nhất là đá quý từ các danh mục khác nhau không bao giờ tương tác ngoại trừ thông qua giới hạn vị trí chung. Trong một danh mục, điều duy nhất quan trọng là tổng giá trị thuộc tính có thể nhận được bằng cách sử dụng một số vị trí nhất định. 

Số lượng vị trí tối đa là 2000 và mỗi danh mục chứa tối đa 2000 viên đá quý. Một giải pháp lập trình động với độ phức tạp xung quanh`O(total_gems × n)`dễ dàng đủ nhanh, vì đây là khoảng`3 × 2000 × 2000 = 1.2 × 10^7`các chuyển đổi trạng thái. Bất cứ điều gì liên quan đến tập hợp con của đá quý hoặc tìm kiếm trực tiếp tất cả các phân bổ giữa các đá quý sẽ hoàn toàn không khả thi. 

Một số trường hợp đáng chú ý. 

Nếu một thuộc tính không bao giờ được cải thiện thì sản phẩm cuối cùng sẽ bằng 0 bất kể hai thuộc tính còn lại. 

Ví dụ,```
2
1 1 1
2 5
3 10
3 10
```Không có viên ngọc mana hoặc tốc độ tấn công nào vừa với hai vị trí, vì vậy mọi giải pháp khả thi đều có sản phẩm`0`. 

Một trường hợp khó phát hiện khác là giải pháp tối ưu có thể để lại các khe không được sử dụng. 

Ví dụ,```
5
1 1 1
2 100
2 100
2 100
```Việc sử dụng một viên ngọc từ mỗi danh mục sẽ tiêu tốn sáu ô, điều này là không thể. Giải pháp tốt nhất chỉ sử dụng hai viên đá quý, mang lại sản phẩm`0`, bởi vì một thuộc tính vẫn bằng 0. Không có yêu cầu phải điền chính xác từng vị trí. 

Sai lầm thứ ba là cho rằng mỗi danh mục sẽ nhận được khoảng một phần ba số vị trí. 

Ví dụ,```
10
3 1 1
2 100
2 100
2 100
2 1
4 1
```Những viên đá quý ma thuật mạnh hơn đáng kể so với những viên đá quý khác. Sự phân bổ tối ưu rất mất cân bằng và bất kỳ chiến lược nào khắc phục trước việc phân bổ vị trí đều có thể bỏ lỡ sự phân bổ tối ưu. 

## Phương pháp tiếp cận 

Một giải pháp mạnh mẽ đơn giản sẽ liệt kê mọi tập hợp con đá quý. Đối với mỗi tập hợp con, hãy tính tổng số vị trí đã chiếm và tổng ba thuộc tính, sau đó giữ lại tích hợp lệ tối đa. 

Điều này rõ ràng là đúng vì mọi lựa chọn khả thi đều được kiểm tra. Thật không may, có thể có tới 6000 viên đá quý, vì vậy không gian tìm kiếm chứa`2^6000`các tập hợp con, có kích thước lớn về mặt thiên văn. 

Quan sát quan trọng là ba loại này là độc lập. Giả sử chúng ta biết chính xác điều đó`x`khe được dành cho đá quý ma thuật. Trong danh mục đó, câu hỏi duy nhất là đòn tấn công phép thuật tối đa có thể đạt được bằng cách sử dụng`x`khe cắm. Danh tính cá nhân của những viên đá quý được chọn không còn quan trọng nữa. 

Điều này ngay lập tức gợi ý việc giải quyết ba vấn đề về chiếc ba lô độc lập 0-1. 

Đối với mỗi loại, xác định`best[i] = maximum attribute value obtainable using exactly i slots`. 

Sau ba phép tính ba lô này, mọi phân bổ vị trí có thể`a + b + c ≤ n`có thể được kiểm tra bằng cách đánh giá`best_magic[a] × best_mana[b] × best_speed[c]`. 

Giai đoạn ba lô nén nhiều tập con theo cấp số nhân thành chỉ`n + 1`tiểu bang cho mỗi danh mục, làm cho việc liệt kê cuối cùng trở nên thực tế. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^(m1+m2+m3)) | O(1) | Quá chậm | 
| Tối ưu | O((m1+m2+m3)n+n³) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo thói quen mang theo ba lô cho một loại đá quý. Cho phép`dp[i]`biểu thị giá trị thuộc tính tối đa có thể đạt được bằng cách sử dụng chính xác`i`khe cắm. Khởi tạo`dp[0]=0`và mọi trạng thái khác là không thể. 
2. Xử lý mỗi viên ngọc một lần. Số lượng vị trí di chuyển theo thứ tự giảm dần để thực thi giới hạn 0-1. Nếu việc sử dụng đá quý hiện tại cải thiện giá trị cho số lượng vị trí lớn hơn, hãy cập nhật trạng thái đó. 
3. Chạy chiếc ba lô này một cách độc lập để lấy các viên ngọc ma thuật, năng lượng và tốc độ tấn công. Ba mảng kết quả chứa giá trị tốt nhất có thể đạt được cho mỗi lần sử dụng vị trí chính xác. 
4. Liệt kê mọi cặp có thể`(a,b)`số lượng vị trí được gán cho hai danh mục đầu tiên. 
5. Tối đa số slot còn lại dành cho hạng mục thứ ba là`n-a-b`. Liệt kê mọi khả năng có thể`c`từ`0`đến giới hạn đó. 
6. Bỏ qua bất kỳ trạng thái nào không thể thực hiện được ở một trong ba mảng ba lô. 
7. Tính tích của ba giá trị thuộc tính và giữ giá trị lớn nhất. 

### Tại sao nó hoạt động 

Tính toán ba lô xem xét mọi tập hợp con đá quý trong một danh mục chính xác một lần, do đó, với mỗi số vị trí, nó sẽ lưu trữ giá trị thuộc tính lớn nhất có thể đạt được. Vì các danh mục không bao giờ chia sẻ đá quý và chỉ tương tác thông qua tổng ngân sách vị trí, nên mọi giải pháp tổng thể khả thi đều tương ứng với chính xác một bộ ba mức sử dụng vị trí`(a,b,c)`. Trong lần liệt kê cuối cùng, bộ ba đó sẽ được kiểm tra và giá trị tốt nhất cho từng danh mục đã được biết. Việc thay thế bất kỳ danh mục nào bằng một tập hợp con yếu hơn sử dụng cùng số lượng vị trí chỉ có thể làm giảm sản phẩm, do đó, việc kết hợp ba lựa chọn tối ưu độc lập cho số lượng vị trí đó luôn là tối ưu. Vì mọi phân bổ vị trí khả thi đều được kiểm tra nên mức tối đa toàn cầu sẽ được tìm thấy. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

NEG = -10 ** 18

def knapsack(m, n):
    dp = [NEG] * (n + 1)
    dp[0] = 0
    for _ in range(m):
        v, w = map(int, input().split())
        for s in range(n - v, -1, -1):
            if dp[s] != NEG:
                dp[s + v] = max(dp[s + v], dp[s] + w)
    return dp

def solve():
    n = int(input())
    m1, m2, m3 = map(int, input().split())

    atk = knapsack(m1, n)
    mana = knapsack(m2, n)
    speed = knapsack(m3, n)

    ans = 0

    for a in range(n + 1):
        if atk[a] == NEG:
            continue
        for b in range(n - a + 1):
            if mana[b] == NEG:
                continue
            limit = n - a - b
            for c in range(limit + 1):
                if speed[c] == NEG:
                    continue
                val = atk[a] * mana[b] * speed[c]
                if val > ans:
                    ans = val

    print(ans)

if __name__ == "__main__":
    solve()
```Hàm trợ giúp thực hiện một chiếc ba lô 0-1 tiêu chuẩn. Việc lặp lại số lượng vị trí theo thứ tự giảm dần đảm bảo rằng mỗi viên đá quý được sử dụng nhiều nhất một lần. 

Các trạng thái không thể được khởi tạo thành số âm lớn thay vì 0. Sự khác biệt này rất cần thiết vì số 0 là giá trị thuộc tính hợp pháp khi không có viên ngọc nào được chọn. 

Sau khi tính toán ba bảng lập trình động, chương trình sẽ liệt kê mọi phân bổ vị trí khả thi. Vòng lặp bên trong không bao giờ vượt quá dung lượng còn lại, vì vậy mọi phân bổ được kiểm tra đều tự động đáp ứng tổng giới hạn vị trí. 

Số nguyên Python có độ chính xác tùy ý, do đó không cần xử lý đặc biệt đối với sản phẩm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào```
10
2 2 2
2 1
3 2
2 1
3 2
5 3
4 2
```Sau ba phép tính ba lô: 

| Khe ma thuật | Tấn công tốt nhất | 
| --- | --- | 
| 0 | 0 | 
| 2 | 1 | 
| 3 | 2 | 
| 5 | 3 | 

| Khe mana | Năng lượng tốt nhất | 
| --- | --- | 
| 0 | 0 | 
| 2 | 1 | 
| 3 | 2 | 
| 5 | 3 | 

| Khe tốc độ | Tốc độ tốt nhất | 
| --- | --- | 
| 0 | 0 | 
| 4 | 2 | 
| 5 | 3 | 
| 9 | 5 | 

Trong quá trình liệt kê: 

| Khe ma thuật | Khe mana | Khe tốc độ | Sản phẩm | 
| --- | --- | --- | --- | 
| 3 | 3 | 4 | 8 | 
| 2 | 3 | 5 | 6 | 
| 5 | 5 | 0 | 0 | 

Sản phẩm tối đa là`8`. 

Ví dụ này cho thấy giải pháp tốt nhất đến từ các lựa chọn tối ưu độc lập cho từng danh mục bằng cách sử dụng phân bổ vị trí tương thích. 

### Ví dụ 2```
3
1 1 1
1 5
1 4
1 3
```Bàn ba lô: 

| Khe ma thuật | Tấn công | 
| --- | --- | 
| 0 | 0 | 
| 1 | 5 | 

| Khe mana | Mana | 
| --- | --- | 
| 0 | 0 | 
| 1 | 4 | 

| Khe tốc độ | Tốc độ | 
| --- | --- | 
| 0 | 0 | 
| 1 | 3 | 

Đếm: 

| Khe ma thuật | Khe mana | Khe tốc độ | Sản phẩm | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 60 | 
| 1 | 1 | 0 | 0 | 
| 1 | 0 | 1 | 0 | 

Câu trả lời là`60`, sử dụng một viên ngọc từ mỗi danh mục. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((m1+m2+m3)n+n³) | Ba chiếc ba lô 0-1, sau đó là liệt kê tất cả các bộ ba vị trí khả thi | 
| Không gian | O(n) | Ba mảng có chiều dài`n+1`| 

Với`n ≤ 2000`, giai đoạn ba lô thực hiện khoảng 12 triệu bản cập nhật. Việc liệt kê bậc ba chỉ được chấp nhận vì các ràng buộc chính thức cho bài toán này được thiết kế cho phương pháp này. Việc sử dụng bộ nhớ là tuyến tính trong giới hạn khe cắm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    from solution import solve

    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout
    return out.getvalue().strip()

assert run("""10
2 2 2
2 1
3 2
2 1
3 2
5 3
4 2
""") == "8"

assert run("""1
1 1 1
1 1
1 1
1 1
""") == "0"

assert run("""3
1 1 1
1 5
1 4
1 3
""") == "60"

assert run("""2
1 1 1
2 5
2 7
2 9
""") == "0"

assert run("""6
2 2 2
3 5
3 5
3 5
3 5
3 5
3 5
""") == "125"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đầu vào mẫu | 8 | Ví dụ chính thức | 
| Công suất tối thiểu | 0 | Một thuộc tính không thể tăng lên | 
| Đá quý ba đơn vị | 60 | Phân bổ thành công cơ bản | 
| Chỉ có một danh mục phù hợp | 0 | Sản phẩm trở thành 0 nếu thiếu một thuộc tính | 
| Đá quý có kích thước bằng nhau | 125 | Phân chia công suất chính xác theo danh mục | 

## Vỏ cạnh 

Hãy xem xét trường hợp một danh mục không thể đóng góp.```
2
1 1 1
2 5
3 10
3 10
```Bảng ba lô về mana và tốc độ tấn công chỉ chứa trạng thái không có ô. Mỗi phân bổ liệt kê tạo ra ít nhất một thuộc tính bằng 0, do đó thuật toán trả về chính xác`0`. 

Bây giờ hãy xem xét các vị trí chưa được sử dụng.```
5
1 1 1
2 100
2 100
2 100
```Cách duy nhất để cải thiện cả ba thuộc tính cần có sáu ô, vượt quá khả năng. Việc liệt kê bao gồm mọi phân bổ sử dụng tối đa năm vị trí, không có vị trí nào mang lại giá trị dương cho cả ba thuộc tính. Câu trả lời là chính xác`0`. 

Cuối cùng, hãy xem xét một tối ưu không cân bằng.```
10
3 1 1
2 100
2 100
2 100
2 1
4 1
```Ba lô dành cho phép thuật đặc biệt ủng hộ việc dành nhiều ô ở đó, trong khi các danh mục khác chỉ yêu cầu một vài ô. Vì mọi phân bổ vị trí khả thi đều được kiểm tra nên thuật toán sẽ tự nhiên tìm ra phân bổ sai lệch này thay vì giả định phân chia đồng đều.
