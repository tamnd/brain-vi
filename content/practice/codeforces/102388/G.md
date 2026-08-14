---
title: "CF 102388G - Ốc Sên"
description: "Chúng tôi theo dõi một con ốc bắt đầu ở độ sâu n, trong đó độ sâu 0 có nghĩa là nó đã chạm tới mặt đất. Mỗi ngày nó leo lên một mét. Nếu lần leo đó chạm tới hoặc vượt qua mặt đất, con ốc sên sẽ trốn thoát ngay lập tức và quá trình kết thúc."
date: "2026-08-12T21:16:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102388
codeforces_index: "G"
codeforces_contest_name: "SUFE ICPC Team Formation Test"
rating: 0
weight: 102388
solve_time_s: 426
verified: true
draft: false
---

[CF 102388G - Ốc sên](https://codeforces.com/problemset/problem/102388/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 7 phút 6 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi theo dõi một con ốc bắt đầu từ độ sâu`n`, ở đâu độ sâu`0`nghĩa là nó đã chạm tới mặt đất. Trong mỗi ngày nó leo lên`a`mét. Nếu lần leo đó chạm tới hoặc vượt qua mặt đất, con ốc sên sẽ trốn thoát ngay lập tức và quá trình kết thúc. Nếu không, đêm đến và con ốc rơi`b`mét, do đó độ sâu của nó tăng thêm`b`. 

Đối với mỗi trường hợp thử nghiệm, nhiệm vụ là xác định ngày đầu tiên con ốc sên chạm đất. Nếu con ốc sên không bao giờ trốn thoát được, chúng ta sẽ xuất ra`-1`. 

Các ràng buộc rất nhỏ: có tối đa 10 trường hợp thử nghiệm và`n`,`a`, Và`b`tất cả đều tối đa là 1000. Do đó, một mô phỏng trực tiếp thực hiện tối đa khoảng 1000 chu kỳ ngày cho một trường hợp thử nghiệm khi có thể thoát, dễ dàng nằm trong giới hạn 1 giây. Mặc dù vậy, cấu trúc của quy trình mang lại cho chúng ta công thức O(1), công thức này sạch hơn và vẫn hiệu quả nếu các giới hạn được tăng lên đáng kể. 

Điều khó khăn là con ốc sên không bị ngã sau lần leo núi cuối cùng. Ví dụ, với`n = 5, a = 5, b = 100`, câu trả lời là`1`, vì ốc sên chạm đất vào ngày đầu tiên. Việc thực hiện bất cẩn luôn áp dụng vào lúc màn đêm buông xuống trước khi kiểm tra xem con ốc có trốn thoát hay không có thể báo nhầm rằng nó vẫn còn trong giếng. 

Một trường hợp khác là khi leo và rơi đều lớn như nhau. Vì`n = 10, a = 5, b = 5`, con ốc leo từ độ sâu 10 lên 5, tụt xuống độ sâu 10 và lặp lại mãi mãi. Đầu ra đúng là`-1`. Chỉ kiểm tra xem`a`khác 0 sẽ bỏ lỡ tình huống này. 

Vụ án`a < b`cũng là điều không thể trừ khi con ốc sên chạm đất vào ngày đầu tiên. Ví dụ,`n = 6, a = 3, b = 4`cho`-1`. Sau mỗi ngày không thành công, con ốc lại càng lún sâu hơn trước nên không bao giờ có thể hồi phục được nữa. 

Cuối cùng,`a = 0`phải được xử lý một cách tự nhiên. Ví dụ,`n = 1, a = 0, b = 0`sản xuất`-1`, bởi vì con ốc sên không bao giờ di chuyển chút nào. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất mô phỏng mỗi ngày. Chúng tôi duy trì độ sâu hiện tại, trừ`a`trong ngày và kiểm tra ngay xem độ sâu đã đạt đến 0 chưa. Nếu không, chúng tôi thêm`b`qua đêm và tiếp tục. Điều này tuân theo chính xác quy trình vật lý, vì vậy bất cứ khi nào quá trình mô phỏng kết thúc với một lần thoát thì ngày được báo cáo là chính xác. 

Mô phỏng cũng phải phát hiện một quá trình không thể thực hiện được. Sau một ngày đêm không thành công, độ sâu thay đổi theo`a - b`theo hướng đi lên. Nếu như`a <= b`, con ốc sên không bao giờ gần mặt đất hơn sau khi hoàn thành trọn một ngày đêm. Nếu nó không thoát được vào ngày đầu tiên thì nó sẽ không bao giờ thoát được. 

Theo những ràng buộc nhất định, vũ lực đã đủ nhanh. Khi`a > b`, tiến độ thực nhỏ nhất có thể là một mét mỗi ngày trọn vẹn và độ sâu ban đầu nhiều nhất là 1000. Do đó, một mô phỏng thành công cần tối đa khoảng 1000 lần lặp cho mỗi trường hợp thử nghiệm hoặc khoảng 10000 lần lặp trên tất cả các trường hợp thử nghiệm. Không cần phải từ chối phương pháp này đối với các giới hạn thực tế. 

Quan sát hữu ích hơn là sau mỗi ngày không thành công, độ sâu của ốc sên giảm đi chính xác`a - b`. Giả sử con ốc sên không trốn thoát vào ngày đầu tiên. Trước ngày cuối cùng, nó đã hoàn thành nhiều chu kỳ cả ngày lẫn đêm, mỗi chu kỳ đều mang lại tiến độ thực sự như nhau. Chúng ta có thể tính toán trực tiếp cần bao nhiêu chu kỳ như vậy thay vì mô phỏng chúng. 

Nếu con ốc bắt đầu một ngày ở độ sâu`d`, nó thoát khỏi ngày hôm đó đúng vào lúc`d <= a`. Trong ngày đầu tiên, điều này có nghĩa là`n <= a`, đưa ra câu trả lời ngay lập tức`1`. Ngược lại, sau mỗi ngày hoàn toàn không thành công, độ sâu sẽ giảm đi`a - b`. Nếu như`a <= b`, không có tiến triển tích cực nên việc trốn thoát là không thể. Nếu như`a > b`, chúng ta chỉ cần chia trần để xác định cần thêm bao nhiêu chu kỳ. 

Phương pháp brute-force hoạt động vì mỗi lần lặp đại diện cho một ngày thực. Nó trở nên kém hấp dẫn hơn khi`n`tăng lên vì thời gian chạy của nó tỷ lệ thuận với số ngày. Quan sát thấy rằng mỗi ngày không thành công đều thay đổi độ sâu theo cùng một lượng cố định cho phép chúng ta thay thế tất cả các thao tác lặp lại đó bằng một phép tính số học. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) trong trường hợp xấu nhất | O(1) | Được chấp nhận cho các ràng buộc nhất định | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Trước tiên hãy kiểm tra xem`n <= a`. Con ốc sên chạm đất trong lần leo đầu tiên vào ban ngày nên câu trả lời là ngay lập tức`1`. Mùa thu vào ban đêm không liên quan vì quá trình này dừng lại ngay khi chạm tới mặt đất. 
2. Nếu ngày đầu tiên không thoát được, hãy kiểm tra xem`a <= b`. Sau đó, mọi chu kỳ ngày đêm hoàn chỉnh sẽ không tiến lên hoặc đẩy con ốc xuống sâu hơn. Vì ngày đầu tiên đã thất bại nên ốc sên không thể trốn thoát nên hãy quay lại`-1`. 
3. Bây giờ chúng ta đã biết`a > b`, do đó, mỗi ngày trọn vẹn không thành công sẽ làm giảm độ sâu đi`a - b`. Cho phép`k`là số ngày sau đó ốc sên trốn thoát. Sau đó`k - 1`hoàn thành chu kỳ ngày đêm, độ sâu của nó là`n - (k - 1)(a - b)`. 

Vào ngày`k`, nó leo lên`a`mét, vì vậy việc trốn thoát đòi hỏi`n - (k - 1)(a - b) <= a`. 

Sắp xếp lại mang lại`n - a <= (k - 1)(a - b)`. 

Do đó, số chu kỳ không thành công hoàn chỉnh cần thiết là`ceil((n - a) / (a - b))`. 

Ngày leo núi cuối cùng cộng thêm một ngày nữa nên đáp án là`ceil((n - a) / (a - b)) + 1`. 
4. Tính số chia trần bằng phép tính số nguyên:`(x + y - 1) // y`tích cực`x`Và`y`, Ở đâu`x = n - a`Và`y = a - b`. Điều này tránh số học dấu phẩy động và đưa ra câu trả lời số nguyên chính xác. 

**Tại sao nó hoạt động.** Trước ngày cuối cùng, mỗi chu kỳ ngày-đêm hoàn thành sẽ thay đổi chính xác độ sâu của ốc sên`a - b`. Khi`a <= b`, sự thay đổi đó không thể đưa con ốc lên trên nên sau khi thất bại trong ngày đầu tiên việc trốn thoát là không thể. Khi`a > b`, độ sâu sau bất kỳ số chu kỳ hoàn chỉnh nào được xác định chính xác bằng cùng một mức giảm cố định. Công thức tìm ra số chu kỳ hoàn chỉnh nhỏ nhất mà lần leo núi tiếp theo vào ban ngày có thể chạm tới mặt đất. Vì phép tính sử dụng giá trị nhỏ nhất nên nó cho kết quả chính xác là ngày trốn thoát đầu tiên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n, a, b = map(int, input().split())

        if n <= a:
            print(1)
            continue

        if a <= b:
            print(-1)
            continue

        net = a - b
        remaining = n - a

        days = (remaining + net - 1) // net + 1
        print(days)

if __name__ == "__main__":
    solve()
```Chương trình trước tiên xử lý trường hợp thoát khỏi ngày đầu tiên với`n <= a`. Việc kiểm tra này phải xảy ra trước điều kiện không thể thực hiện được vì ốc sên có thể trốn thoát ngay cả khi`a <= b`. Ví dụ,`n = 5, a = 5, b = 100`có câu trả lời`1`. 

Sau đó,`a <= b`có nghĩa là mọi chu trình hoàn chỉnh đều không cải thiện được độ sâu của ốc sên. Vì con ốc sên không trốn thoát trong ngày đầu tiên nên câu trả lời là`-1`. 

Đối với các trường hợp còn lại,`net = a - b`là hoàn toàn tích cực. Biến`remaining = n - a`thể hiện mức độ tiến bộ bổ sung cần thiết trước khi chuyến leo núi ban ngày có thể kết thúc cuộc trốn thoát. Bộ phận trần nhà tính toán cần bao nhiêu chu kỳ ngày đêm trước lần leo núi cuối cùng đó. 

trận chung kết`+ 1`tượng trưng cho ngày trốn thoát. Đây là điểm khác biệt phổ biến nhất trong vấn đề. Chúng tôi đếm các chu kỳ hoàn chỉnh trước khi leo lên thành công, không phải là leo lên thành công. 

Số nguyên Python không tràn cho các ràng buộc này và tất cả các phép tính đều sử dụng số học số nguyên, do đó không có vấn đề về độ chính xác. 

## Ví dụ đã hoạt động 

### Mẫu 1, test case đầu tiên 

cho`n = 10, a = 5, b = 2`, con ốc sên không thể trốn thoát vào ngày 1 vì nó leo từ độ sâu 10 lên độ sâu 5. Sau đêm nó rơi trở lại độ sâu 7. Quá trình tương tự lặp lại cho đến ngày thứ 3. 

| Ngày | Độ sâu khi bắt đầu | Sau khi leo | Bỏ trốn? | Độ sâu sau đêm | 
| --- | --- | --- | --- | --- | 
| 1 | 10 | 5 | Không | 7 | 
| 2 | 7 | 2 | Không | 4 | 
| 3 | 4 | -1 | Có | Không áp dụng | 

Tiến trình thực của mỗi chu kỳ không thành công là`5 - 2 = 3`. Công thức cho`ceil((10 - 5) / 3) + 1 = ceil(5 / 3) + 1 = 2 + 1 = 3`. 

Ngày thứ ba thành công và việc thu đêm không bao giờ được thực hiện vì con ốc sên đã trốn thoát. 

### Mẫu 1, test case thứ hai 

cho`n = 10, a = 5, b = 5`, lần leo đầu tiên khiến con ốc ở độ sâu 5, và màn đêm đưa nó trở lại độ sâu 10. Trạng thái lặp lại mãi mãi. 

| Ngày | Độ sâu khi bắt đầu | Sau khi leo | Bỏ trốn? | Độ sâu sau đêm | 
| --- | --- | --- | --- | --- | 
| 1 | 10 | 5 | Không | 10 | 
| 2 | 10 | 5 | Không | 10 | 
| 3 | 10 | 5 | Không | 10 | 

Đây`a <= b`, do đó thuật toán trả về ngay`-1`. Dấu vết chứng minh tại sao việc kiểm tra tiến trình thực là đủ để phát hiện một quá trình vô hạn. 

### Mẫu 1, test case thứ ba 

cho`n = 5, a = 5, b = 6`, con ốc sên chạm đất trong lần leo lên đầu tiên. 

| Ngày | Độ sâu khi bắt đầu | Sau khi leo | Bỏ trốn? | Độ sâu sau đêm | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | 0 | Có | Không áp dụng | 

Câu trả lời là`1`, cho dù`a < b`. Đây chính xác là lý do tại sao việc kiểm tra ngày đầu tiên phải diễn ra trước khi kiểm tra khả năng không thể thực hiện được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T) | Mỗi trường hợp thử nghiệm chỉ yêu cầu một số phép tính số học không đổi. | 
| Không gian | O(1) | Chỉ một số biến số nguyên cố định được lưu trữ cho mỗi trường hợp thử nghiệm. | 

Với tối đa 10 ca kiểm thử, chương trình chỉ thực hiện một số phép tính số học cho mỗi ca kiểm thử. Nó thoải mái ở cả giới hạn thời gian 1 giây và giới hạn bộ nhớ 256 MB. Không giống như mô phỏng, thời gian chạy không phụ thuộc vào số ngày con ốc cần trốn thoát. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline
    t = int(input())

    for _ in range(t):
        n, a, b = map(int, input().split())

        if n <= a:
            print(1)
        elif a <= b:
            print(-1)
        else:
            net = a - b
            remaining = n - a
            print((remaining + net - 1) // net + 1)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()
    output = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return output

# Provided sample
assert run("""3
10 5 2
10 5 5
5 5 6
""") == """3
-1
1
""", "sample 1"

# Minimum-size values
assert run("""1
1 0 0
""") == """-1
""", "no movement is impossible"

# Immediate escape even though the night fall is larger
assert run("""1
5 5 100
""") == """1
""", "escape before night"

# Exact escape after several cycles
assert run("""1
10 4 1
""") == """4
""", "exact day calculation"

# Maximum-size values with minimal positive net progress
assert run("""1
1000 1000 1000
""") == """1
""", "maximum values and immediate escape"

# Large case requiring many days, net progress is exactly one
assert run("""1
1000 2 1
""") == """999
""", "maximum number of simulated days"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0 0`|`-1`| Không leo và không rơi nên ốc sên không bao giờ di chuyển. | 
|`5 5 100`|`1`| Việc thoát hiểm ngay lập tức phải được kiểm tra trước`a <= b`. | 
|`10 4 1`|`4`| Kiểm tra việc phân chia trần và đếm ngày cuối cùng. | 
|`1000 1000 1000`|`1`| Giá trị tối đa thoát vào ngày đầu tiên. | 
|`1000 2 1`|`999`| Một trường hợp có tiến độ ròng dương nhỏ nhất và nhiều ngày cần thiết. | 

## Vỏ cạnh 

Con ốc sên có thể trốn thoát trong ngày đầu tiên thì câu trả lời luôn là`1`, bất chấp màn đêm buông xuống. Đối với đầu vào`5 5 100`, đầu tiên thuật toán sẽ kiểm tra`5 <= 5`và trả về`1`. Một giải pháp kiểm tra`a <= b`đầu tiên sẽ tuyên bố không chính xác rằng quá trình này là không thể, mặc dù con ốc sên không bao giờ đến vào ban đêm. 

Khi`a = b`và ngày đầu tiên thất bại, con ốc sên trở lại độ sâu như cũ sau mỗi đêm. Vì`10 5 5`, lần leo đầu tiên thay đổi độ sâu từ 10 thành 5, sau đó màn đêm trở về 10. Thuật toán nhìn thấy`n > a`Và`a <= b`, vì vậy nó trả về`-1`mà không cần thử mô phỏng vô hạn. 

Khi`a < b`, tình hình còn tệ hơn sau ngày đầu tiên không thành công. Vì`6 3 4`, lần leo đầu tiên thay đổi độ sâu từ 6 đến 3, sau đó vào ban đêm thay đổi thành 7. Mỗi chu kỳ tiếp theo bắt đầu sâu hơn chu kỳ trước. Vì lần leo đầu tiên không đủ nên thuật toán trả về`-1`. 

Khi`a = 0`, con ốc sên không thể leo lên được chút nào. Vì`1 0 0`, điều kiện ngày đầu tiên`1 <= 0`là sai, và`a <= b`là đúng, vậy câu trả lời là`-1`. Nếu như`b`dương thì tình huống đó cũng không thể xảy ra vì con ốc chỉ di chuyển xuống dưới. 

Trường hợp ranh giới chính xác xảy ra khi số chu kỳ cần thiết chia tiến trình ròng một cách hoàn hảo. Vì`n = 7, a = 3, b = 1`, ngày đầu tiên kết thúc ở độ sâu 4, sau đó đêm đầu tiên lên đến độ sâu 5. Ngày thứ hai kết thúc ở độ sâu 2, rồi đêm thứ hai lên đến độ sâu 3. Lần leo thứ ba chạm tới mặt đất. Công thức cho`ceil((7 - 3) / 2) + 1 = 2 + 1 = 3`, do đó không có thêm ngày nào được đưa ra bởi cách tính trần. 

Ranh giới ngày cuối cùng cũng là lý do tại sao công thức sử dụng`n - a`, thay vì chỉ đơn giản là`n`. Con ốc sên cần phải ở đủ gần để nó có thể leo trèo vào ban ngày và hoàn thành công việc. Đếm đêm sau khi leo thành công sẽ thêm một chu kỳ bổ sung không chính xác.
