---
title: "CF 105862A - Rồng"
description: "Chúng ta được cung cấp một mô phỏng tiến trình đơn giản liên quan đến một nhân vật và một chuỗi các con rồng. Mỗi con rồng có một sức mạnh cần thiết để đánh bại và phần thưởng về sức mạnh sau khi bị đánh bại. Ký tự bắt đầu bằng giá trị cường độ ban đầu."
date: "2026-06-25T14:34:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105862
codeforces_index: "A"
codeforces_contest_name: "ACPC Kickoff 2025"
rating: 0
weight: 105862
solve_time_s: 48
verified: true
draft: false
---

[CF 105862A - Rồng](https://codeforces.com/problemset/problem/105862/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mô phỏng tiến trình đơn giản liên quan đến một nhân vật và một chuỗi các con rồng. Mỗi con rồng có một sức mạnh cần thiết để đánh bại và phần thưởng về sức mạnh sau khi bị đánh bại. Ký tự bắt đầu bằng giá trị cường độ ban đầu. Những con rồng có thể được chiến đấu theo bất kỳ thứ tự nào và mục tiêu là xác định xem liệu có tồn tại một thứ tự trong đó tất cả các con rồng có thể bị đánh bại tuần tự để trong mỗi cuộc chiến, sức mạnh hiện tại của nhân vật vượt quá yêu cầu của con rồng hay không. 

Dữ liệu đầu vào mô tả một sức mạnh ban đầu và sau đó là danh sách các cuộc chạm trán với rồng, mỗi lần được biểu thị bằng hai con số: sức mạnh tối thiểu cần thiết để chiến đấu với con rồng đó và sức mạnh có được sau khi chiến thắng. Nhiệm vụ là quyết định xem có thể đánh bại tất cả các con rồng hay không và đưa ra câu trả lời có hoặc không tương ứng. 

Các ràng buộc thường đủ lớn để loại trừ mọi cách tiếp cận thử mọi hoán vị của rồng. Nếu có n con rồng, một giải pháp dựa trên hoán vị ngây thơ sẽ tiêu tốn n phép toán giai thừa, điều này trở nên không khả thi ngay cả với n khoảng 20. Điều này ngay lập tức đẩy chúng ta tới một chiến lược đặt hàng tham lam với việc sắp xếp, chiến lược này diễn ra thoải mái trong thời gian n log n. 

Một chế độ thất bại tinh vi sẽ xuất hiện nếu chúng ta cố gắng chiến đấu với rồng theo thứ tự đầu vào nhất định mà không có lý do về thứ tự. Một ví dụ nhỏ minh họa vấn đề. Giả sử sức mạnh ban đầu là 10, còn rồng là (sức mạnh 11, phần thưởng 5) và (sức mạnh 10, phần thưởng 100). Nếu làm theo thứ tự đầu vào, chúng ta sẽ thất bại ngay lập tức vì 10 không đủ cho 11. Tuy nhiên, nếu lần đầu tiên chiến đấu với con rồng thứ hai, chúng ta sẽ có được sức mạnh lớn và sau đó có thể đánh bại con rồng thứ nhất. Điều này cho thấy rằng việc ra lệnh là khó khăn cốt lõi chứ không phải bản thân việc mô phỏng chiến đấu. 

## Phương pháp tiếp cận 

Chiến lược vũ phu là thử mọi hoán vị có thể có của rồng và mô phỏng các trận chiến theo thứ tự đó. Đối với mỗi hoán vị, chúng tôi kiểm tra xem sức mạnh của nhân vật có còn đủ ở mỗi bước hay không. Điều này hiệu quả vì nó khám phá toàn diện tất cả các trình tự có thể có, đảm bảo rằng nếu một đơn hàng hợp lệ tồn tại thì nó sẽ được tìm thấy. 

Vấn đề với cách tiếp cận này là số lượng hoán vị. Với n con rồng thì có n! các đơn hàng có thể có và với mỗi đơn hàng, chúng tôi thực hiện công việc mô phỏng O(n). Ngay cả với n = 15, điều này đã vượt quá giới hạn thực tế và với n = 20, điều đó hoàn toàn không khả thi. 

Điểm quan trọng là quyết định ra lệnh chỉ phụ thuộc vào sức mạnh hiện tại so với yêu cầu của rồng và phần thưởng luôn tăng sức mạnh. Cấu trúc này gợi ý rằng việc giải quyết các yêu cầu yếu hơn sớm hơn sẽ mở rộng nhóm rồng mà chúng ta có thể xử lý sau này. Nếu chúng ta sắp xếp các con rồng theo sức mạnh cần thiết theo thứ tự tăng dần, chúng ta đảm bảo rằng chúng ta không bao giờ trì hoãn một con rồng “dễ” trong khi thử sớm một con rồng khó hơn. Sau khi được sắp xếp, một đường tuyến tính duy nhất là đủ để mô phỏng quá trình. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! · n) | O(n) | Quá chậm | 
| Sắp xếp tham lam | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên chúng tôi sắp xếp lại rồng để những con có yêu cầu sức mạnh nhỏ hơn sẽ đến sớm hơn. Thứ tự này rất quan trọng vì nó sắp xếp trình tự phù hợp với những gì hiện có thể thực hiện được ở mỗi bước.

1. Sắp xếp tất cả rồng theo sức mạnh yêu cầu theo thứ tự tăng dần. Điều này đảm bảo chúng tôi luôn xem xét thử thách yếu nhất hiện có trước tiên. 
2. Khởi tạo cường độ dòng điện bằng giá trị ban đầu. 
3. Lặp lại từng con rồng đã được sắp xếp. Ở mỗi con rồng, hãy so sánh sức mạnh hiện tại với yêu cầu của nó. 
4. Nếu sức mạnh hiện tại lớn hơn yêu cầu, hãy đánh bại con rồng và tăng sức mạnh hiện tại bằng giá trị phần thưởng. 
5. Nếu tại bất kỳ thời điểm nào cường độ hiện tại không đủ, hãy chấm dứt sớm và kết luận là thất bại, vì không có yêu cầu nào sau này có thể khắc phục được thực tế là chúng tôi đã bị chặn bởi một yêu cầu yếu hơn những gì chúng tôi có thể xử lý. 
6. Nếu tất cả rồng đều được xử lý thành công, xuất ra thành công. 

Tại sao sự lựa chọn tham lam lại có giá trị xuất phát từ thực tế là việc đánh bại bất kỳ con rồng nào có thể tiếp cận được không bao giờ làm giảm sức mạnh mà chỉ mở rộng khả năng trong tương lai. Việc sắp xếp đảm bảo rằng bất cứ khi nào một con rồng bị bỏ qua, đó không phải là do thứ tự mà là do điều đó thực sự là không thể vào thời điểm đó. 

Điều bất biến được duy trì là trước khi xử lý từng con rồng theo thứ tự được sắp xếp, chúng tôi đã đánh bại tất cả những con rồng có yêu cầu nhỏ hơn hoặc bằng nhau có thể đánh bại và sức mạnh hiện tại của chúng tôi là mức tối đa có thể đạt được theo tiền tố đó. Điều này ngăn cản việc bỏ lỡ bất kỳ trận đấu sớm có lợi nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s, n = map(int, input().split())
    dragons = []

    for _ in range(n):
        x, y = map(int, input().split())
        dragons.append((x, y))

    dragons.sort(key=lambda d: d[0])

    for x, y in dragons:
        if s > x:
            s += y
        else:
            print("NO")
            return

    print("YES")

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách đọc sức mạnh ban đầu và số lượng rồng. Chúng tôi lưu trữ mỗi con rồng như một cặp yêu cầu và phần thưởng. Sắp xếp theo yêu cầu là bước quan trọng giúp chuyển một bài toán tổ hợp thành một bài toán mô phỏng tuyến tính. 

Trong quá trình lặp lại, chúng tôi thực thi điều kiện so sánh nghiêm ngặt`s > x`. Sự nghiêm ngặt này rất quan trọng vì sự bình đẳng không được phép; thiếu điều kiện này là một lỗi thường gặp. Việc thoát sớm khi xảy ra lỗi sẽ tránh được việc tính toán không cần thiết nhưng không cần thiết để đảm bảo tính chính xác mà chỉ cần hiệu quả. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
10 2
11 5
10 100
```Rồng được sắp xếp: 

| Bước | Sức mạnh | Rồng (yêu cầu, phần thưởng) | Hành động | Sức mạnh mới | 
| --- | --- | --- | --- | --- | 
| 1 | 10 | (10, 100) | 10 > 10 sai | dừng lại | 

Quá trình dừng lại ngay lập tức vì sự bình đẳng chưa đủ để giành chiến thắng. Điều này cho thấy tại sao chỉ đặt hàng không giải quyết được mọi thứ nếu chúng ta bỏ qua điều kiện bất đẳng thức nghiêm ngặt. 

### Ví dụ 2 

đầu vào:```
10 3
5 2
9 1
8 10
```Rồng được sắp xếp: 

| Bước | Sức mạnh | Rồng | Hành động | Sức mạnh mới | 
| --- | --- | --- | --- | --- | 
| 1 | 10 | (5, 2) | chiến đấu | 12 | 
| 2 | 12 | (8, 10) | chiến đấu | 22 | 
| 3 | 22 | (9, 1) | chiến đấu | 23 | 

Mọi con rồng đều trở nên dễ xử lý hơn sau khi đạt được lợi ích sớm, điều này xác nhận rằng thứ tự yêu cầu ngày càng tăng sẽ duy trì tính khả thi đồng thời tối đa hóa các cơ hội phát triển sớm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | phân loại chiếm ưu thế, mô phỏng là tuyến tính | 
| Không gian | O(n) | lưu trữ danh sách rồng | 

Thuật toán phù hợp một cách thoải mái trong các ràng buộc điển hình trong đó n có thể đạt tới 10^5, vì việc sắp xếp và một lần vượt qua là đủ hiệu quả trong các giới hạn tiêu chuẩn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdout = old_stdout
    return out.strip()

# provided sample-like case
assert run("10 2\n11 5\n10 100\n") == "NO"

# all possible successful
assert run("10 1\n5 10\n") == "YES"

# must reorder to succeed
assert run("10 3\n11 1\n5 1\n8 1\n") == "YES"

# impossible case
assert run("1 2\n2 1\n3 1\n") == "NO"

# edge: exact equality failure
assert run("5 1\n5 100\n") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trường hợp tối thiểu | CÓ | rồng đơn thành công | 
| đặt hàng hỗn hợp | CÓ | phân loại cần thiết | 
| chuỗi bất khả thi | KHÔNG | phát hiện lỗi sớm | 
| cạnh bình đẳng | KHÔNG | quy tắc bất bình đẳng nghiêm ngặt | 

## Vỏ cạnh 

Trường hợp một cạnh là khi sức mạnh ban đầu bằng đúng với yêu cầu của rồng. Đối với đầu vào`s = 5`và rồng`(5, 100)`, kết quả đúng là thất bại vì điều kiện hoàn toàn lớn hơn. The algorithm handles this correctly through the`if s > x`kiểm tra, ngay lập tức từ chối con rồng. 

Một trường hợp khác là rồng yếu xuất hiện muộn trong đầu vào nhưng phải đánh sớm. Ví dụ: bắt đầu sức mạnh 10 với rồng`(11, 1)`Và`(5, 1)`yêu cầu sắp xếp. Sau khi sắp xếp,`(5, 1)`được xử lý đầu tiên, tăng sức mạnh và cho phép tiến triển. Nếu không sắp xếp, thuật toán sẽ thất bại một cách không chính xác, nhưng đường tham lam được sắp xếp đảm bảo thứ tự chính xác. 

Trường hợp cuối cùng là khi tất cả các con rồng đều không thể thực hiện được nhưng lại có thể thực hiện được sau phần thưởng trước đó. Cấu trúc tham lam giải quyết vấn đề này một cách tự nhiên bởi vì mọi cuộc chiến thành công chỉ làm tăng sức mạnh, đảm bảo không có cơ hội nào trong tương lai bị chặn một cách giả tạo một khi nó có sẵn.
