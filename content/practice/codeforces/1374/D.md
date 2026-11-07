---
problem: 1374D
contest_id: 1374
problem_index: D
name: "Zero Remainder Array"
contest_name: "Codeforces Round 653 (Div. 3)"
rating: 1400
tags: ["math", "sortings", "two pointers"]
answer: passed_samples
verified: true
solve_time_s: 102
date: 2026-06-14
model: gpt-5-5
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2e5b90-5328-83ec-aa40-c68158cde90a
---

#CF 1374D - Mảng không dư 

**Đánh giá:** 1400 
**Tags:** toán, sắp xếp, hai con trỏ 
**Mẫu:** gpt-5-5 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2e5b90-5328-83ec-aa40-c68158cde90a 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên dương. Chúng ta có thể thực hiện một chuỗi các bước di chuyển và mỗi bước di chuyển sẽ nâng cao một bộ đếm tổng thể`x`bắt đầu từ số không. Ở mỗi bước chúng ta bỏ qua hoặc “sử dụng” giá trị hiện tại của`x`trên chính xác một vị trí mảng, thêm`x`tới phần tử đó. Sau một trong hai lựa chọn,`x`tăng lên một. Mỗi chỉ mục có thể nhận thao tác “sử dụng” này nhiều nhất một lần. 

Mục tiêu là làm cho mọi phần tử mảng chia hết cho`k`. Chúng tôi muốn số lần di chuyển tối thiểu cho đến khi điều kiện này được giữ. 

Một cách hữu ích để giải thích quá trình này là thời gian tiến triển một cách xác định thông qua các giá trị của`x = 0, 1, 2, ...`. Tại mỗi thời điểm, chúng tôi có thể gán giá trị hiện tại cho một chỉ mục chưa sử dụng, nhưng cũng được phép bỏ qua. Mỗi chỉ mục, nếu được chọn, sẽ nhận được chính xác một phần bổ sung có dấu thời gian và giá trị phần bổ sung đó sẽ được cố định khi chúng tôi chọn nó. 

Ràng buộc`n ≤ 2·10^5`trên tất cả các trường hợp thử nghiệm ngụ ý rằng chúng ta cần tuyến tính gần đúng hoặc`O(n log n)`hành vi tồi tệ nhất cho mỗi trường hợp thử nghiệm. Bất cứ điều gì giống như kiểm tra tất cả lịch trình hoặc DP theo thời gian đều không thể thực hiện được vì`x`có thể tăng lớn tùy ý và độ dài mô phỏng không bị giới hạn ở dạng đơn giản. 

Một vấn đề tế nhị nảy sinh từ lối suy nghĩ tham lam: việc chọn chỉ số càng sớm càng tốt không phải lúc nào cũng tối ưu, bởi vì các giá trị sau này của`x`lớn hơn và có thể “hữu ích” hơn đối với một số dư lượng. Ví dụ: việc trì hoãn một lựa chọn có thể làm giảm tổng số lần di chuyển cần thiết nếu nhiều chỉ số có thể được cố định cùng nhau ở cùng một bước. 

Một trường hợp khác là khi một số phần tử đã chia hết cho`k`. Những điều này không yêu cầu thao tác, nhưng chúng vẫn tương tác với việc lập kế hoạch chỉ ở chỗ chúng ta không nên chỉ định cho chúng bất kỳ động thái nào. 

Cuối cùng, quá trình chỉ phụ thuộc vào dư lượng modulo`k`, vì chúng ta chỉ quan tâm đến khả năng chia hết. Các giá trị lớn có thể giảm xuống`a_i % k`và mỗi thao tác thêm một dấu thời gian nguyên duy nhất. 

## Phương pháp tiếp cận 

Một cách diễn giải thô bạo sẽ mô phỏng thời gian theo từng bước. Tại mỗi`x`, chúng tôi quyết định có nên gán nó cho một số chỉ mục chưa hoàn thành mà phần thiếu hụt còn lại có thể được thỏa mãn hay không, theo dõi tất cả các khả năng. Điều này nhanh chóng trở thành cấp số nhân vì ở mỗi bước có nhiều lựa chọn về việc áp dụng chỉ mục nào`x`tới và bỏ qua cũng phân nhánh không gian trạng thái. Ngay cả một mô phỏng tham lam cố gắng luôn chỉ định khi có thể cũng thất bại vì tính khả dụng trong tương lai của số lượng lớn`x`các giá trị ảnh hưởng đến việc ghép nối tối ưu. 

Quan sát quan trọng là đảo ngược quan điểm. Thay vì nghĩ đến việc gán dấu thời gian cho các chỉ mục, chúng tôi nghĩ đến việc mỗi chỉ mục cần bao nhiêu “gia số tăng thêm” để đạt được bội số của`k`. Mỗi chỉ số đều có mức thâm hụt còn lại bắt buộc`d_i = (k - a_i % k) % k`. Nếu như`d_i = 0`, nó đã được thực hiện rồi. 

Bây giờ hãy quan sát nhiệm vụ của một bài tập: chọn thời gian`x`đóng góp chính xác`x`đơn vị đối với mức thâm hụt của một chỉ số. Vì vậy, mỗi chỉ mục cần phải được khớp với một số nguyên duy nhất`x`sao cho tổng số đã chọn`x`các giá trị được gán cho nó bằng với mức thâm hụt của nó. Vì mỗi chỉ mục chỉ nhận một giá trị nên về cơ bản mỗi chỉ mục được gán một số duy nhất. 

Điều này biến vấn đề thành việc nhóm các “nhu cầu” giống hệt nhau có thể được thỏa mãn bằng cách chọn cùng một`x`. Tuy nhiên, có một sự tương tác quan trọng: nếu nhiều chỉ số có cùng mức thâm hụt`d`, chúng ta không thể luôn gán chúng giống nhau`x`tự do vì`x`mang tính toàn cầu và ngày càng gia tăng. Nếu chúng ta chỉ định cùng một lớp còn lại nhiều lần, chúng ta phải đảm bảo tồn tại đủ các bước thời gian trong đó`x % k`phù hợp với cấu trúc yêu cầu. 

Sự đơn giản hóa quan trọng đến từ sự thâm hụt ghép đôi: nếu một giá trị thâm hụt`d`cần thiết nhiều lần, chiến lược tối ưu là sắp xếp chúng vào các thời điểm tạo thành một chuỗi tuần hoàn modulo`k`và chi phí phụ thuộc vào số lần lặp lại xảy ra. Cụ thể, đối với tần số`f`của một lớp còn lại nhất định, chúng ta cần`f`số lần xuất hiện cách nhau bởi`k`, dẫn đến sự xuất hiện cuối cùng ở vị trí`d + (f-1)·k`. 

Vì vậy, đối với mỗi mức thâm hụt riêng biệt`d`, chúng tôi tính toán lần cuối cùng chúng tôi cần tiếp cận nhóm đó. Câu trả lời là mức tối đa trên tất cả các nhóm của thời gian yêu cầu cuối cùng này cộng với một (vì các bước di chuyển là các bước thời gian được lập chỉ mục 0). 

Điều này biến vấn đề thành việc đếm tần suất thâm hụt và tính toán một công thức đơn giản cho mỗi nhóm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force | Hàm mũ | O(n) | Quá chậm | 
| Công thức tần suất + lập kế hoạch | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Tính phần thiếu hụt cho từng phần tử như sau`d = (k - a_i % k) % k`. Điều này cho biết mỗi phần tử vẫn cần bao nhiêu để đạt được bội số của`k`. Các phần tử với`d = 0`bị bỏ qua vì chúng không yêu cầu hoạt động. 
2. Đếm xem có bao nhiêu phần tử chia sẻ mỗi giá trị thiếu hụt. Chúng tôi lưu trữ điều này trong một bản đồ tần số. 
3. Đối với mỗi giá trị thâm hụt`d`với tần số`f`, tính thời gian cần thiết để hoàn thành tất cả các phần tử trong nhóm đó là`time = d + (f - 1) * k`. Lý do là sau khi sử dụng một giá trị đóng góp`d`, lần xuất hiện có thể sử dụng tiếp theo của cùng một cấu trúc sẽ xuất hiện mỗi`k`các bước do hạn chế về chu kỳ modulo. 
4. Theo dõi tối đa`time`trên tất cả các nhóm thâm hụt. 
5. Nếu không có thâm hụt khác 0 thì câu trả lời là 0. Nếu không thì câu trả lời là`max_time + 1`, vì chúng ta đếm các bước di chuyển bắt đầu từ`x = 0`. 

### Tại sao nó hoạt động 

Mỗi chỉ số thâm hụt`d`phải nhận chính xác một giá trị được đánh dấu thời gian góp phần sửa chữa phần còn lại của nó. Bởi vì dấu thời gian tăng nghiêm ngặt và bao bọc theo modulo`k`, sự xuất hiện của các đóng góp tương thích lặp lại mỗi`k`các bước. Nếu xuất hiện giá trị thâm hụt`f`Đôi khi, chúng tôi không thể chỉ định tất cả chúng trước khi`(f-1)`-lần lặp lại của lớp modulo đó, buộc phép gán cuối cùng phải xảy ra ít nhất`d + (f-1)·k`. Vì chúng ta chỉ cần thời gian hoàn thành muộn nhất trong tất cả các nhóm nên giới hạn tối đa này sẽ cho số bước di chuyển tối thiểu cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))

        freq = {}
        for v in a:
            r = v % k
            if r == 0:
                continue
            d = k - r
            freq[d] = freq.get(d, 0) + 1

        if not freq:
            print(0)
            continue

        ans = 0
        for d, f in freq.items():
            last_time = d + (f - 1) * k
            ans = max(ans, last_time)

        print(ans + 1)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách nén từng số vào phần điều chỉnh phần dư cần thiết của nó. Chỉ những điều chỉnh khác 0 mới quan trọng vì những phần tử đó đã hợp lệ. Sau đó, chúng tôi tổng hợp số lượng yếu tố cần điều chỉnh tương tự. 

Tính toán chính là`d + (f - 1) * k`, mã hóa thực tế là những khoản thâm hụt giống hệt nhau sẽ cạnh tranh để giành được cùng một loại dư lượng theo thời gian. trận chung kết`+1`tính đến việc chuyển đổi chỉ mục lần cuối thành số lần di chuyển. 

Phải cẩn thận để không đặt lại tần số trên mỗi modulo dư một cách không chính xác, vì`d`không phải`r`, nhưng nó bổ sung cho`k`. Trộn những thứ này dẫn đến việc phân nhóm không chính xác. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai trường hợp: một trường hợp nhỏ và một trường hợp trong đó việc phân nhóm đóng vai trò quan trọng. 

### Ví dụ 1 

đầu vào:```
n=4, k=3
a = [1,2,1,3]
```Thâm hụt: 

| tôi | a_i % 3 | d = (3 - r) % 3 | 
| --- | --- | --- | 
| 1 | 1 | 2 | 
| 2 | 2 | 1 | 
| 3 | 1 | 2 | 
| 4 | 0 | 0 | 

Bản đồ tần số trở thành`{2:2, 1:1}`. 

Bây giờ hãy tính thời gian: 

| d | f | lần cuối = d + (f-1)*k | 
| --- | --- | --- | 
| 2 | 2 | 2 + 3 = 5 | 
| 1 | 1 | 1 | 

Tối đa là`5`, câu trả lời là`6`. 

Điều này phù hợp với ý kiến ​​cho rằng nhóm thâm hụt 2 bị hạn chế hơn vì nó xuất hiện hai lần. 

### Ví dụ 2 

đầu vào:```
n=5, k=4
a = [1,1,1,1,1]
```Tất cả đều có số dư 1 nên tổng số thâm hụt là 3. 

Bản đồ tần số`{3:5}`. 

| d | f | lần cuối cùng | 
| --- | --- | --- | 
| 3 | 5 | 3 + 16 = 19 | 

Câu trả lời là`20`. 

Điều này cho thấy rằng các yêu cầu giống nhau lặp đi lặp lại về khoảng cách lực lượng của`k`giữa các nhiệm vụ hiệu quả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi phần tử được xử lý một lần và tần số được tổng hợp trong bản đồ băm | 
| Không gian | O(n) | Lưu trữ tối đa một mục nhập tần số cho mỗi giá trị thâm hụt riêng biệt | 

Các ràng buộc cho phép lên đến`2·10^5`tổng các phần tử, vì vậy một giải pháp tuyến tính là đủ. Cách tiếp cận bản đồ băm tránh mọi sự phụ thuộc vào`k`, có thể lớn bằng`10^9`. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys

    input = sys.stdin.readline
    t = int(input())
    out = []

    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))

        freq = {}
        for v in a:
            r = v % k
            if r == 0:
                continue
            d = k - r
            freq[d] = freq.get(d, 0) + 1

        if not freq:
            out.append("0")
            continue

        ans = 0
        for d, f in freq.items():
            ans = max(ans, d + (f - 1) * k)

        out.append(str(ans + 1))

    return "\n".join(out)

# provided samples
assert run("""5
4 3
1 2 1 3
10 6
8 7 1 8 3 7 5 10 8 9
5 10
20 100 50 20 100500
10 25
24 24 24 24 24 24 24 24 24 24
8 8
1 2 3 4 5 6 7 8
""") == """6
18
0
227
8"""

# all zero remainder
assert run("""1
3 5
5 10 15
""") == "0"

# single element
assert run("""1
1 7
1
""") == "6"

# all same remainder
assert run("""1
4 3
1 1 1 1
""") == "7"

# mixed residues
assert run("""1
5 4
1 2 3 4 5
""") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều chia hết | 0 | không cần thao tác | 
| phần tử đơn | câu trả lời nhỏ | độ đúng cơ sở | 
| số dư giống nhau | lập kế hoạch cách nhau | hiệu ứng tần số | 
| dư lượng hỗn hợp | nhiều nhóm | logic tổng hợp tối đa | 

## Vỏ cạnh 

Khi tất cả các phần tử đã được chia hết cho`k`, mọi phần còn lại đều bằng 0 và bản đồ tần số vẫn trống. Thuật toán xuất ra số 0 một cách chính xác mà không cần bước vào giai đoạn lập kế hoạch. 

Khi chỉ có một phần tử có số dư khác 0, câu trả lời sẽ trở thành`d + 1`, từ`f = 1`loại bỏ thuật ngữ khoảng cách. Điều này xác nhận rằng không có độ trễ nhân tạo nào được đưa ra bởi logic nhóm. 

Khi nhiều yếu tố có cùng mức thâm hụt, công thức sẽ buộc tăng trưởng tuyến tính theo thời gian, phù hợp với ràng buộc là các nhu cầu giống nhau sẽ cạnh tranh để giành được các dấu thời gian sẵn có liên tiếp, cách nhau bởi`k`.
