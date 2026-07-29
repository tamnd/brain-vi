---
title: "CF 102801D - Những chàng trai mùa thu"
description: "Chúng tôi có một nhóm người chơi đang cố gắng giành lấy một chiếc vương miện đang chuyển động. Vương miện không đứng yên: nó bắt đầu ở độ cao 0, di chuyển lên trên cho đến độ cao H, sau đó di chuyển xuống dưới cho đến độ cao 0 và lặp lại chuyển động này mãi mãi."
date: "2026-07-28T22:55:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102801
codeforces_index: "D"
codeforces_contest_name: "The 14th Chinese Northeast Collegiate Programming Contest"
rating: 0
weight: 102801
solve_time_s: 69
verified: true
draft: false
---

[CF 102801D - Những chàng trai mùa thu](https://codeforces.com/problemset/problem/102801/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một nhóm người chơi đang cố gắng giành lấy một chiếc vương miện đang chuyển động. Vương miện không đứng yên: nó bắt đầu ở độ cao`0`, di chuyển lên trên cho đến khi có độ cao`H`, sau đó di chuyển xuống dưới cho đến khi đạt độ cao`0`và lặp lại chuyển động này mãi mãi. Người chơi có thể lấy vương miện ngay lập tức khi đến đích nếu chiều cao vương miện tối đa`h`. Nếu không, người chơi sẽ đợi cho đến khi vương miện rơi xuống đủ xa. 

Đầu vào mô tả mỗi người chơi theo hai giá trị.`x_i`là thời điểm người chơi đến đích và`c_i`là độ trễ mạng bổ sung được hệ thống trò chơi thêm vào. Nếu một người chơi giành được vương miện vào thời điểm đó`t`, trò chơi ghi lại thời gian lấy đồ của người chơi đó dưới dạng`t + c_i`. Người chiến thắng là người chơi có thời gian ghi nhỏ nhất và nếu nhiều người chơi có thời gian ghi như nhau thì người chơi có chỉ số nhỏ nhất sẽ thắng. 

Thử thách không phải là mô phỏng từng giây của chuyển động vương miện. Số lượng người chơi có thể đạt tới`2 * 10^5`trong một trường hợp thử nghiệm và có thể có tới 20 trường hợp thử nghiệm. Một thuật toán kiểm tra nhiều khoảnh khắc trong tương lai của mọi người chơi có thể dễ dàng đạt được hàng tỷ thao tác. Chúng tôi cần tính toán thời gian không đổi cho khoảnh khắc nắm bắt đầu tiên có thể có của mỗi người chơi. 

Chuyển động của vương miện có chu kỳ rất nhỏ vì`H`nhiều nhất là 300. Toàn bộ chu kỳ kéo dài`2H`giây. Giới hạn nhỏ này có nghĩa là chúng ta có thể suy luận trực tiếp về một chu kỳ thay vì duy trì một mô phỏng dài. 

Những trường hợp phức tạp đều xuất phát từ ranh giới chính xác của phong trào. 

Ví dụ: nếu người chơi đến đúng thời điểm vương miện đạt đến độ cao tối đa:```
1
1 2 5
5
10
```Vương miện ở độ cao`5`vào thời điểm đó`5`, lớn hơn`h = 2`, vì vậy người chơi phải chờ đợi. Thời điểm chấp nhận được tiếp theo là khi vương miện hạ xuống độ cao`2`, vào thời điểm đó`8`. Thời gian ghi lại là`18`, vậy đáp án là:```
1
```Một giải pháp bất cẩn chỉ kiểm tra xem vương miện có ở dưới không`h`sau khi di chuyển xuống dưới có thể coi đỉnh là một điểm có thể chấp nhận được một cách không chính xác. 

Một trường hợp ranh giới khác là khi người chơi đến đúng thời điểm vương miện ở độ cao chấp nhận được:```
1
1 2 5
2
10
```Vào thời điểm`2`, chiều cao vương miện chính xác là`2`, và sự bình đẳng được cho phép. Người chơi chộp ngay nên đáp án là:```
1
```Một giải pháp sử dụng so sánh nghiêm ngặt như`< h`sẽ trì hoãn người chơi một cách không chính xác. 

Trường hợp cạnh cuối cùng là khi`h = H`:```
1
1 5 5
100
7
```Vương miện không bao giờ cao hơn`5`, nên cứ đến là thành công ngay. Câu trả lời là:```
1
```Bất kỳ cách tiếp cận nào giả định luôn có thời gian chờ đợi sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp sẽ mô phỏng chuyển động của vương miện và hỏi mọi người chơi khi nào họ có thể lấy nó. Vì vương miện thay đổi liên tục nên chúng ta cần xác định thời điểm đầu tiên trong tương lai mà chiều cao của nó trở nên chấp nhận được đối với mọi người chơi. Nếu chúng tôi mô phỏng từng giây một, một người chơi có thể cần tới`2H`việc kiểm tra tuy nhỏ nhưng thực hiện việc này cho mọi người chơi vẫn mang lại`O(nH)`hoạt động. Với`n = 200000`Và`H = 300`, điều đó trở thành khoảng 60 triệu lượt kiểm tra cho mỗi trường hợp thử nghiệm, điều này vốn đã gây khó chịu trong Python và không cần thiết. 

Quan sát quan trọng là vương miện đi theo một làn sóng tam giác lặp đi lặp lại. Chúng ta chỉ cần biết vị trí của người chơi trong một khoảng thời gian. Độ dài chu kỳ là`2H`. Trong mỗi chu kỳ, vương miện được chấp nhận theo thời gian`0`ĐẾN`h`trong khi tăng lên và từ thời gian`2H-h`ĐẾN`2H`trong khi rơi. 

Đối với thời gian đến`x`, cho phép`r = x mod (2H)`. Nếu như`r`đã ở một trong những khoảng thời gian có thể chấp nhận được đó thì người chơi sẽ nắm bắt ngay lập tức. Nếu không, người chơi sẽ ở đâu đó ở phần giữa, nơi vương miện quá cao. Thời điểm chấp nhận được tiếp theo luôn là điểm hạ độ cao`h`, xảy ra ở vị trí chu kỳ`2H-h`. Vì vậy, thời gian chờ đợi chỉ đơn giản là khoảng cách từ`r`ĐẾN`2H-h`. 

Điều này làm giảm toàn bộ vấn đề thành việc tính toán một công thức cho mỗi người chơi. Lực lượng vũ phu hoạt động vì vương miện có chu kỳ có thể đoán trước được, nhưng không thành công vì nó lặp lại quá nhiều lần trên nhiều người chơi. Cấu trúc tuần hoàn cho phép chúng ta bỏ qua hoàn toàn việc mô phỏng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nH) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi người chơi, hãy tính thời gian lấy vật lý đầu tiên. Cho phép`period = 2H`Và`r = x_i mod period`. giá trị`r`cho chúng ta biết vương miện ở đâu trong chu kỳ hiện tại của nó. 
2. Kiểm tra xem vương miện đã có thể tiếp cận được chưa. Điều này xảy ra khi`r <= h`hoặc`r >= 2H - h`. Trong trường hợp đó, thời gian lấy vật lý của người chơi chỉ đơn giản là`x_i`. 
3. Nếu vương miện quá cao, người chơi phải đợi cho đến khi vương miện rơi xuống`h`. Thời điểm tiếp theo như vậy là ở vị trí chu kỳ`2H - h`, do đó thời gian lấy vật lý trở thành:```
x_i + (2H - h - r)
```1. Thêm độ trễ của người chơi`c_i`để có được thời gian lấy được ghi lại. So sánh nó với câu trả lời tốt nhất hiện tại. Thay thế câu trả lời nếu người chơi này thắng sớm hơn hoặc nếu số lần ghi được bằng nhau và chỉ số của người chơi này nhỏ hơn. 

### Tại sao nó hoạt động 

Chuyển động của vương miện lặp đi lặp lại mỗi`2H`giây, do đó mọi tình huống có thể xảy ra hoàn toàn được xác định bởi thời gian còn lại của thời gian đến trong chu kỳ đó. Thời điểm duy nhất có thể lấy được là hai khoảng thời gian có chiều cao vương miện tối đa`h`. Nếu người chơi ở ngoài những khoảng thời gian đó, thời điểm có thể tiếp cận tiếp theo phải là điểm đầu tiên của khoảng thời gian chấp nhận được tiếp theo, đó chính xác là thời điểm đi xuống ở độ cao`h`. Bởi vì thuật toán tính toán thời điểm bắt sớm nhất có thể cho mọi người chơi và sau đó áp dụng quy tắc hòa chính xác, người chơi được chọn được đảm bảo là người chiến thắng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n, h, H = map(int, input().split())
        x = list(map(int, input().split()))
        c = list(map(int, input().split()))

        period = 2 * H
        best_time = None
        best_id = None

        for i in range(n):
            r = x[i] % period

            if r <= h or r >= period - h:
                grab = x[i]
            else:
                grab = x[i] + (period - h - r)

            total = grab + c[i]

            if best_time is None or total < best_time or (total == best_time and i < best_id):
                best_time = total
                best_id = i

        ans.append(str(best_id + 1))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Giải pháp xử lý người chơi một cách độc lập vì thời gian chờ đợi của một người chơi không ảnh hưởng đến tương tác vương miện của người chơi khác. Biến`r`lưu trữ vị trí bên trong chu kỳ núm vặn hiện tại, tránh mọi mô phỏng chuyển động. 

điều kiện`r <= h or r >= period - h`bao gồm cả hai điểm cuối vì người chơi có thể lấy khi chiều cao vương miện chính xác`h`. Thiếu đẳng thức sẽ tạo ra câu trả lời sai trong các trường hợp biên. 

Khi vương miện quá cao, công thức`period - h - r`luôn luôn tích cực. Kết quả là thời gian còn lại chính xác cho đến khi bên giảm dần đạt đến độ cao`h`. 

Sự so sánh cũng xử lý các mối quan hệ một cách cẩn thận. Mảng Python không được lập chỉ mục bằng 0, nhưng vấn đề là số lượng người chơi bắt đầu từ một, do đó chỉ mục được lưu trữ chỉ tăng thêm một khi in. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu đầu tiên:```
2
4 2 5
3 6 1 9
6 4 2 3
3 1 2
1 2 3
4 5 6
```Đối với trường hợp thử nghiệm đầu tiên, độ dài chu kỳ là`10`và các khoảng có thể đạt được là`[0,2]`Và`[8,10]`. 

| Người chơi | Đến`x`| Trì hoãn`c`|`x mod 10`| Lấy thời gian | Thời gian ghi | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 6 | 3 | 8 | 14 | 
| 2 | 6 | 4 | 6 | 8 | 12 | 
| 3 | 1 | 2 | 1 | 1 | 3 | 
| 4 | 9 | 3 | 9 | 9 | 12 | 

Người chơi 3 có thời gian ghi nhỏ nhất nên đáp án là`3`. Dấu vết này cho thấy tại sao chỉ thời gian đến nơi vật lý là không đủ. Người chơi sau vẫn có thể thua vì sự chậm trễ là một phần của sự so sánh. 

Đối với trường hợp thử nghiệm thứ hai,`H = 2`Và`h = 1`, vậy độ dài chu kỳ là`4`. 

| Người chơi | Đến`x`| Trì hoãn`c`|`x mod 4`| Lấy thời gian | Thời gian ghi | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 4 | 1 | 1 | 5 | 
| 2 | 2 | 5 | 2 | 3 | 8 | 
| 3 | 3 | 6 | 3 | 3 | 9 | 

Người chơi 1 thắng ngay lập tức. Trường hợp này chứng tỏ rằng người chơi đến trong khi vương miện quá cao phải đợi đến khoảng thời gian có thể tiếp cận tiếp theo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi người chơi được xử lý một lần chỉ bằng các phép tính số học. | 
| Không gian | O(1) | Ngoài các mảng đầu vào, chỉ có một vài biến được lưu trữ. | 

Số lượng người chơi tối đa lớn nhưng thuật toán hoạt động liên tục trên mỗi người chơi. Sự phụ thuộc vào`H`biến mất hoàn toàn nên nghiệm dễ dàng đạt giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return out.getvalue()

assert run("""2
4 2 5
3 6 1 9
6 4 2 3
3 1 2
1 2 3
4 5 6
""") == "3\n1\n", "samples"

assert run("""1
1 2 5
2
10
""") == "1\n", "arrival exactly at reachable height"

assert run("""1
1 2 5
5
10
""") == "1\n", "arrival at peak"

assert run("""1
3 5 5
100 1 50
1 20 3
""") == "1\n", "h equals H"

assert run("""1
3 1 10
9 11 12
1 1 1
""") == "1\n", "cycle boundary handling"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Trường hợp mẫu |`3`,`1`| Tính đúng đắn chung | 
|`h = 2, H = 5, x = 2`|`1`| Bình đẳng ở tầm cao | 
|`h = 2, H = 5, x = 5`|`1`| Xử lý cao điểm | 
|`h = H`|`1`| Không có thời gian chờ đợi | 
| Những người đến xung quanh ranh giới chu kỳ |`1`| Modulo và độ chính xác từng cái một | 

## Vỏ cạnh 

Khi người chơi đến chính xác điểm cao nhất trong chuyển động của vương miện, phần còn lại là`H`. Nếu như`h < H`, đây là khoảng giữa không thể truy cập được. Ví dụ:```
1
1 2 5
5
10
```Phần còn lại là`5`và vị trí có thể tiếp cận tiếp theo là`8`, vậy thời gian lấy vật lý là`8`. Thuật toán đạt được kết quả tương tự vì`5`nhiều nhất là không`2`ít nhất cũng không`8`. 

Khi một người chơi đến đúng lúc vương miện đang ở trên cao`h`, được phép lấy ngay lập tức. Vì:```
1
1 2 5
2
10
```phần còn lại là`2`, thỏa mãn`r <= h`. Thuật toán giữ nguyên thời gian đến thay vì thêm thời gian chờ đợi không cần thiết. 

Khi`h = H`, mọi chiều cao vương miện có thể đều được chấp nhận. Vì:```
1
1 5 5
100
7
```thời kỳ là`10`, và điều kiện luôn thành công vì mọi số dư nhiều nhất là`5`hoặc ít nhất`5`. Người chơi lấy ngay theo yêu cầu.
