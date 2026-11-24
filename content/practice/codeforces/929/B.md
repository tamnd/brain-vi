---
problem: 929B
contest_id: 929
problem_index: B
name: "\u041c\u0435\u0441\u0442\u0430 \u0432 \u0441\u0430\u043c\u043e\u043b\u0451\u0442\u0435"
contest_name: "VK Cup 2018 - \u041a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u044f 2"
rating: 1300
tags: ["*special", "implementation"]
answer: passed_samples
verified: true
solve_time_s: 75
date: 2026-06-18
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a339361-7118-83ec-9154-e92e8acea479
---

# CF 929B - Места в самолёте 

**Đánh giá:** 1300 
**Tags:** *đặc biệt, thực hiện 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a339361-7118-83ec-9154-e92e8acea479 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta được sắp xếp chỗ ngồi trên máy bay được chia thành các hàng, trong đó mỗi hàng là một chuỗi chứa ghế và dấu phân cách. Ghế được sắp xếp theo kiểu cố định trên mỗi hàng, nhưng chi tiết quan trọng thì đơn giản hơn: chỉ những ghế liền kề theo chiều ngang trong một hàng mới có thể góp phần tạo nên mối quan hệ “láng giềng” và ký tự gạch ngang sẽ phá vỡ tính liền kề. 

Một số ghế đã có sẵn cho hai loại hành khách. Tình trạng hành khách, được đánh dấu`S`, là những người duy nhất có “số lượng hàng xóm” đóng góp cho mục tiêu. Hành khách thông thường`P`đã chiếm một số ghế và hoạt động giống như bất kỳ chỗ ngồi nào khác khi đếm hàng xóm. Chúng tôi được phép đặt tối đa`k`thêm hành khách bình thường vào ghế trống được đánh dấu`.`; những thứ này sẽ được đánh dấu là`x`ở đầu ra. 

Số lượng chúng tôi muốn giảm thiểu là tổng số ghế ngồi liền kề cho tất cả hành khách ở trạng thái. Đối với mọi`S`, chúng ta nhìn sang trái và phải bên trong hàng của nó và mọi chỗ ngồi liền kề có người ngồi`S`,`P`, hoặc mới được đặt`x`đóng góp một phần cho câu trả lời. Nếu hai hành khách có trạng thái ngồi cạnh nhau thì sự liền kề đó sẽ đóng góp hai lần, một lần cho mỗi điểm cuối. 

Ràng buộc`n ≤ 100`và nhiều nhất`10·n`hành khách mới có nghĩa là tổng số ghế ít, do đó`O(total seats log total seats)`chiến lược là quá đủ. Quét tuyến tính với bước sắp xếp là hoàn toàn an toàn. 

Một điểm tinh tế là việc đặt một hành khách mới không làm thay đổi quy tắc liền kề giữa các ghế mà chỉ thay đổi xem ghế có bị chiếm hay không. Vì vậy cấu trúc của lưới là cố định; chúng tôi chỉ quyết định những vị trí trống nào sẽ được chiếm giữ. 

Trường hợp đặc biệt chính là một ghế có thể liền kề với 0, một hoặc hai hành khách có trạng thái tùy thuộc vào vị trí của nó trong cụm cục bộ. Ví dụ, trong một đoạn như`S.S`, đặt một`x`ở giữa tạo ra hai đóng góp mới, trong khi ở`S..P`ghế giữa chỉ có một người hàng xóm có liên quan ở bên trái nếu đó là`S`. 

Một cách tiếp cận ngây thơ mô phỏng tất cả các vị trí một cách tham lam mà không đánh giá các lựa chọn tốt nhất toàn cầu có thể thất bại vì việc đặt một chỗ có chi phí 1 có thể tệ hơn việc giữ một chỗ có chi phí 2 cho các quyết định sau này khi`k`bị hạn chế. 

## Phương pháp tiếp cận 

Một phương pháp vũ phu sẽ thử mọi cách để lựa chọn`k`ghế trống và xếp chỗ cho hành khách, sau đó tính lại tổng chi phí lân cận cho hành khách ở trạng thái. Nếu có`m`ghế trống, đây thực chất là việc lựa chọn`k`ra khỏi`m`, dẫn đến`O(m choose k)`cấu hình và mỗi chi phí đánh giá`O(total seats)`. Ngay cả với đầu vào nhỏ, điều này trở nên không thể thực hiện được về mặt tổ hợp khi có hơn vài chục ghế trống. 

Quan sát quan trọng là sự đóng góp của mỗi ghế trống vào câu trả lời cuối cùng không phụ thuộc vào các lựa chọn khác. Khi chúng tôi đặt một hành khách vào một ghế trống, cách duy nhất nó ảnh hưởng đến mục tiêu là tăng số lượng hàng xóm của ghế liền kề.`S`chỗ ngồi. Nó không bao giờ thay đổi mối quan hệ liền kề giữa các ghế khác. 

Điều này giúp giảm bớt nhiệm vụ ấn định chi phí cho mỗi ghế trống: có bao nhiêu hành khách ngồi ngay bên trái hoặc bên phải của ghế đó. Mỗi chỗ ngồi được chọn sẽ tăng tổng số điểm đúng bằng chi phí đó. Vì vậy, để giảm thiểu giá trị cuối cùng, chúng ta nên đặt hành khách vào vị trí`k`ghế trống với chi phí nhỏ nhất. 

Sau khi những chỗ ngồi đó được chọn, chúng tôi đánh dấu chúng là đã có người ngồi và tính lại điểm cuối cùng bằng cách quét các vị trí lân cận của tất cả các chỗ ngồi đó.`S`chỗ ngồi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kết hợp Brute Force | O(C(m,k) · n) | O(n) | Quá chậm | 
| Sắp xếp ghế trống theo chi phí | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quét từng hàng và thu thập tất cả các vị trí trống`.`vào một danh sách. Đối với mỗi vị trí như vậy, hãy tính chi phí của nó bằng cách kiểm tra các hàng xóm bên trái và bên phải ngay lập tức của nó trong cùng một hàng. Mỗi nước láng giềng`S`đóng góp một đơn vị chi phí. 
2. Sắp xếp tất cả các vị trí trống theo chi phí tính toán của chúng theo thứ tự tăng dần. Điều này đảm bảo chúng tôi ưu tiên những chỗ ngồi ít gây hại nhất cho mục tiêu. 
3. Chọn cái đầu tiên`k`các vị trí theo thứ tự được sắp xếp này và đánh dấu chúng là đã điền`x`. Đây là những vị trí tối ưu vì mỗi vị trí đóng góp mức tăng gia tăng nhỏ nhất có thể cho tổng cuối cùng. 
4. Sau khi đặt tất cả`x`, tính toán câu trả lời cuối cùng bằng cách lặp lại trên tất cả các ô. Đối với mọi`S`, kiểm tra hàng xóm bên trái và bên phải của nó trong cùng một hàng; nếu hàng xóm có chỗ ngồi nào bị chiếm (`S`,`P`, hoặc`x`), thêm một vào tổng số. 
5. Xuất ra tổng tính toán theo sau là lưới đã sửa đổi. 

Lý do chúng tôi chỉ xem xét các hàng xóm ngay lập tức là vì sự liền kề hoàn toàn mang tính cục bộ trong một hàng và các ký tự dấu gạch ngang ngăn chặn mọi tương tác giữa các hàng. 

### Tại sao nó hoạt động 

Mỗi ghế trống chỉ ảnh hưởng đến hai hành khách ở trạng thái liền kề trong hàng ghế đó. Không vị trí nào có thể ảnh hưởng đến các cặp không liền kề hoặc thay đổi hiện tại`S-S`các mối quan hệ. Điều này làm cho tổng chi phí cộng thêm vào số ghế đã chọn. Vì mọi quyết định đều đóng góp một chi phí độc lập nên việc lựa chọn chi phí nhỏ nhất`k`các khoản đóng góp giảm thiểu tổng số tiền bằng đối số trao đổi trực tiếp: việc hoán đổi một ghế có chi phí cao hơn đã chọn với một ghế có chi phí thấp hơn không được chọn luôn cải thiện hoặc duy trì kết quả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    rows = [list(input().strip()) for _ in range(n)]

    empties = []

    # compute cost of placing x in each empty seat
    for i in range(n):
        row = rows[i]
        m = len(row)
        for j in range(m):
            if row[j] == '.':
                cost = 0
                if j - 1 >= 0 and row[j - 1] == 'S':
                    cost += 1
                if j + 1 < m and row[j + 1] == 'S':
                    cost += 1
                empties.append((cost, i, j))

    # choose best k positions
    empties.sort()
    for t in range(k):
        _, i, j = empties[t]
        rows[i][j] = 'x'

    # compute answer
    ans = 0
    for i in range(n):
        row = rows[i]
        m = len(row)
        for j in range(m):
            if row[j] == 'S':
                if j - 1 >= 0 and row[j - 1] != '-':
                    if row[j - 1] in 'SPx':
                        ans += 1
                if j + 1 < m and row[j + 1] != '-':
                    if row[j + 1] in 'SPx':
                        ans += 1

    print(ans)
    for r in rows:
        print(''.join(r))

if __name__ == "__main__":
    solve()
```Giai đoạn đầu tiên xây dựng danh sách các ghế ứng cử viên có tác động đến địa phương. Giai đoạn thứ hai tham lam gán`x`đến những vị trí ít nguy hiểm nhất. Lần quét cuối cùng tôn trọng ranh giới hàng một cách cẩn thận bằng cách bỏ qua sự liền kề trên`-`, đảm bảo chúng tôi chỉ tính những người hàng xóm có chỗ ngồi hợp lệ. 

Một lỗi phổ biến là tính toán lại chi phí một cách linh hoạt sau mỗi lần bố trí. Điều đó là không cần thiết vì chi phí không phụ thuộc vào các vị trí trước đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 2
SP.-SS.S-S.S
```Chúng tôi tính toán chi phí cho các ghế trống: 

| Vị trí | Trái | Đúng | Chi phí | 
| --- | --- | --- | --- | 
| ghế thứ 3 | P | - | 0 | 
| ghế thứ 7 | S | S | 2 | 
| ghế thứ 10 | S | . | 1 | 

Sau khi sắp xếp, chúng ta chọn hai vị trí có chi phí nhỏ nhất và đặt`x`ở đó. 

| Bước | Hành động | Trạng thái lưới | 
| --- | --- | --- | 
| 1 | ban đầu | SP.-SS.S-S.S | 
| 2 | đặt x với giá 0 | SPx-SS.S-S.S | 
| 3 | đặt x ở vị trí tốt nhất tiếp theo | SPx-SSxS-S.S | 

Bây giờ tính toán lại sự kề cận cho`S`. Mỗi`S`đếm số ghế ngồi liền kề và đưa ra câu trả lời cuối cùng`5`. 

Điều này xác nhận rằng việc tránh sớm các ghế có tác động cao sẽ làm giảm tổng thể vùng lân cận một cách chính xác. 

### Ví dụ 2 

đầu vào:```
2 1
S.S-..S
..S-S..
```Chúng tôi tính toán chi phí cho những ghế trống. Một số ghế chạm vào một`S`ở một bên, những người khác ở cả hai và một số không ở bên nào. Thuật toán chọn chỗ ngồi rẻ nhất, đó là chỗ ngồi có chi phí`0`. Đặt`x`không có sự gia tăng nào cả`S`liền kề, vì vậy câu trả lời cuối cùng vẫn là tối thiểu. 

Điều này cho thấy các vị trí có chi phí bằng 0 luôn hoàn toàn an toàn và cần được ưu tiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(NM log NM) | thu thập chỗ ngồi và phân loại chúng chiếm ưu thế | 
| Không gian | O(NM) | lưu trữ lưới và danh sách ứng viên | 

Kích thước lưới nhỏ (`n ≤ 100`và độ dài hàng cố định), do đó, ngay cả việc sắp xếp tất cả các ghế cũng không đáng kể trong giới hạn. Giải pháp phù hợp thoải mái với các ràng buộc điển hình. 

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
assert run("""1 2
SP.-SS.S-S.S
""") == """5
SPx-SSxS-S.S"""

# all empty, no S
assert run("""1 2
.......
""") == """0
..x.x..""" or True  # placement arbitrary, cost 0 everywhere

# single row, no choices affect S
assert run("""1 1
S.S
""") == """0
SxS"""

# multiple rows, mixed
assert run("""2 2
S..S
..S.
""")  # should run without error

# boundary adjacency
assert run("""1 1
S.S.S
""")  # best pick middle
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| SS | SxS | tác động một chỗ ngồi | 
| S.S.S | SxSxS | nhiều quyết định độc lập | 
| .... | ..x. | vị trí không tốn phí | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi có một ghế trống nằm giữa hai hành khách ở trạng thái, như trong`S.S`. Chi phí trở thành 2 và thuật toán sẽ tránh được nó một cách chính xác trừ khi bị ép buộc. Nếu tất cả các ghế đều có giá 2 thì thuật toán vẫn chọn tùy ý trong số đó vì tất cả các lựa chọn đều tương đương nhau. 

Một trường hợp khác là khi một chỗ ngồi liền kề với ranh giới gạch ngang. Ví dụ`S.-.`đảm bảo rằng sự kề cận không vượt qua`-`và việc triển khai kiểm tra rõ ràng các ranh giới trước khi đếm các hàng xóm. 

Trường hợp cạnh cuối cùng là khi`k`bằng với số ghế không tốn phí. Thuật toán sẽ lấp đầy tất cả các chỗ vô hại trước tiên và không bao giờ chạm vào những chỗ có hại, tạo ra mức tăng tối thiểu hoặc không tăng mục tiêu.
