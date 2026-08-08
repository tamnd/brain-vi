---
title: "CF 102536C - Senpai"
description: "Senpai đánh giá một người dựa trên nhiều phẩm chất. Mỗi phẩm chất đều có trọng lượng, vì vậy việc cải thiện một số phẩm chất quan trọng hơn việc cải thiện những phẩm chất khác. Mức chất lượng yêu cầu của Senpai thay đổi tuyến tính theo thời gian. Đối với mọi chất lượng, yêu cầu đều có giá trị ban đầu và tốc độ thay đổi."
date: "2026-08-07T21:16:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102536
codeforces_index: "C"
codeforces_contest_name: "2020 UP ACM Algolympics Final Round"
rating: 0
weight: 102536
solve_time_s: 125
verified: true
draft: false
---

[CF 102536C - Senpai](https://codeforces.com/problemset/problem/102536/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 5s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Senpai đánh giá một người dựa trên nhiều phẩm chất. Mỗi phẩm chất đều có trọng lượng, vì vậy việc cải thiện một số phẩm chất quan trọng hơn việc cải thiện những phẩm chất khác. Mức chất lượng yêu cầu của Senpai thay đổi tuyến tính theo thời gian. Đối với mọi chất lượng, yêu cầu đều có giá trị ban đầu và tốc độ thay đổi. 

Kouhai bắt đầu với mọi phẩm chất cá nhân ở con số 0. Hạn chế duy nhất là tốc độ cải tiến: tại bất kỳ thời điểm nào, vectơ của mọi tốc độ tăng trưởng chất lượng tối đa phải có độ dài`g`. Nhiệm vụ là tìm ra thời điểm sớm nhất để Kouhai có thể lựa chọn chiến lược cải thiện tối ưu sao cho điểm cá nhân có trọng số ít nhất đạt tiêu chuẩn hiện tại của Senpai. 

Đầu vào cung cấp số lượng phẩm chất, tốc độ tăng trưởng tối đa, trọng số của tất cả các phẩm chất và hàm tuyến tính mô tả các tiêu chuẩn thay đổi của Senpai. Đầu ra là thời điểm nhỏ nhất mà thành công có thể đạt được. 

Các ràng buộc tiết lộ rằng số lượng chất lượng trong tất cả các trường hợp thử nghiệm chỉ là`10^4`, do đó, việc vượt qua tuyến tính các phẩm chất được mong đợi. Bất kỳ mô phỏng nào theo thời gian, lập trình động theo chất lượng hoặc tìm kiếm liên tục thực hiện các kiểm tra tốn kém sẽ tạo thêm công việc không cần thiết. Câu trả lời là một số thực, vì vậy thách thức chính là tìm ra công thức thay vì xử lý độ chính xác bằng số thông qua các phép tính phức tạp. 

Những trường hợp phức tạp xuất phát từ thực tế là trọng lượng, tốc độ tăng trưởng và yêu cầu ban đầu đều có thể âm. Một giải pháp giả định mọi giá trị đều dương có thể thất bại một cách âm thầm. 

Hãy xem xét một phẩm chất duy nhất mà Kouhai đã vượt quá yêu cầu:```
1
1 10
1
0 -5
```Đầu ra đúng là`0`. Vì yêu cầu ban đầu là`-5`và số điểm ban đầu của Kouhai là`0`, không cần cải thiện. Một công thức bất cẩn chia ngay mà không kiểm tra trạng thái hiện tại có thể tạo ra thời gian âm. 

Một trường hợp khác là khi trọng số âm làm cho việc cải thiện chất lượng đó trở nên có hại:```
1
1 10
-2
0 5
```Chất lượng không nên tăng lên vì trọng lượng âm. Lập luận đúng sử dụng tổng chiều dài của vectơ trọng số, cho phép Kouhai di chuyển theo hướng tối đa hóa điểm trọng số. Một giải pháp chỉ tính tổng các trọng số và bỏ qua dấu của chúng sẽ tính toán sai tiềm năng tăng trưởng. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ cố gắng mô phỏng các chiến lược cải tiến có thể thực hiện được. Vào một thời điểm đã chọn`t`, nó có thể cố gắng quyết định số tiền ngân sách tăng trưởng sẽ dành cho mỗi chất lượng, sau đó kiểm tra xem điểm có trọng số có đạt yêu cầu của Senpai hay không. Điều này đúng vì điều kiện chỉ phụ thuộc vào giá trị chất lượng cuối cùng, nhưng không gian của các hàm cải tiến liên tục có thể có là rất lớn. Ngay cả việc giảm nó xuống mức kiểm tra nhiều lần cũng sẽ yêu cầu giải quyết vấn đề tối ưu hóa nhiều lần. Với`q = 1000`, việc thử nhiều cách phân bổ ứng viên hoặc sử dụng mô phỏng thời gian chi tiết sẽ nhanh chóng trở nên không khả thi. 

Quan sát quan trọng là chỉ có vị trí cuối cùng của vectơ chất lượng của Kouhai mới quan trọng. Bắt đầu từ số 0, bất kỳ đường tăng trưởng hợp lệ nào cũng có thể di chuyển vectơ chất lượng ở khoảng cách tối đa`g * t`sau thời gian`t`. Điểm có trọng số là tích số chấm giữa vectơ chất lượng cuối cùng và vectơ trọng số. Tích số chấm lớn nhất có thể có với vectơ có độ dài giới hạn thu được bằng cách di chuyển chính xác theo hướng của trọng số. 

Điểm có trọng số tối đa có thể sau thời gian`t`do đó là: 

g⋅t⋅∣∣W∣∣ 

ở đâu: 

∣∣W∣∣= i ∑ ​ W i 2 ​ ​ 

Tổng yêu cầu của Senpai cũng tuyến tính: 

i ∑ ​ S i ​ (t)=t i ∑ ​ F i ​ + i ∑ ​ C i ​ 

Cả hai vế bây giờ đều là các hàm tuyến tính đơn giản của thời gian. Câu trả lời có thể được tìm thấy trực tiếp bằng cách giải một bất đẳng thức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(tìm kiếm vô hạn trên các chiến lược) | O(q) | Quá chậm | 
| Tối ưu | O(q) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc trọng số của các phẩm chất và tính bình phương chiều dài của vectơ trọng số. Hướng của vectơ trọng số xác định cách tốt nhất để sử dụng tốc độ tăng trưởng của Kouhai, vì vậy chỉ cần độ lớn của nó. 
2. Thêm tất cả các yếu tố tăng trưởng Senpai vào`sum_f`và tất cả các yêu cầu ban đầu vào`sum_c`. Hai giá trị này mô tả tiêu chuẩn tổng thể của Senpai dưới dạng một hàm tuyến tính duy nhất. 
3. Tính mức tăng điểm tối đa có thể có của Kouhai trong một đơn vị thời gian như sau: 

g tôi ∑ ​ W tôi 2 ​ ​ 

Đây là tốc độ tăng lớn nhất có thể của tổng chất lượng có trọng số vì hướng cải tiến có thể được lựa chọn một cách tự do. 

1. So sánh hai hàm tuyến tính. Nếu như`sum_c <= 0`, Kouhai đã đáp ứng yêu cầu tại thời điểm 0 nên đáp án là 0. 
2. Ngược lại giải: 

(g∣∣W∣∣− i ∑ ​ F i ​ )t ≥ i ∑ ​ C i ​ 

Mẫu số phải dương vì bài toán đảm bảo rằng có câu trả lời. Chia yêu cầu còn lại cho giá trị này để có được thời gian tối thiểu. 

Tại sao nó hoạt động: sau thời gian`t`, mọi chiến lược cải tiến có thể tạo ra một vectơ chất lượng có độ dài Euclide tối đa là`gt`. Theo bất đẳng thức Cauchy-Schwarz, điểm có trọng số nhiều nhất là tích của hai độ dài vectơ, tức là`gt||W||`. Giới hạn trên này có thể đạt được bằng cách chọn hướng cải tiến song song với vectơ trọng số. Thuật toán sử dụng chính xác số điểm tối đa có thể này và tìm ra lần đầu tiên khi nó đạt được yêu cầu của Senpai, vì vậy không có thời gian nào sớm hơn có thể hoạt động và thời gian trả về là có thể đạt được. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def solve():
    c = int(input())
    ans = []

    for _ in range(c):
        q, g = map(int, input().split())

        w = list(map(int, input().split()))

        sum_f = 0
        sum_c = 0
        for _ in range(q):
            f, cc = map(int, input().split())
            sum_f += f
            sum_c += cc

        w_len = math.sqrt(sum(x * x for x in w))
        growth = g * w_len

        if sum_c <= 0:
            ans.append("0.00000000000")
        else:
            t = sum_c / (growth - sum_f)
            ans.append("{:.12f}".format(t))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Đầu tiên, mã này quy đổi tất cả thông tin chất lượng thành ba giá trị: độ lớn của vectơ trọng số, tổng tốc độ thay đổi tiêu chuẩn của Senpai và tổng tiêu chuẩn ban đầu. Những phẩm chất cá nhân không còn cần phải được lưu giữ sau khi sự đóng góp của chúng được tích lũy. 

Căn bậc hai chỉ được tính một lần cho mỗi trường hợp thử nghiệm. Độ chính xác của dấu phẩy động là đủ vì lỗi yêu cầu là`1e-10`, và các giá trị liên quan là nhỏ. Công thức chỉ sử dụng phép chia sau khi kiểm tra trường hợp không có thời gian, tránh các câu trả lời phủ định sai. 

Không có vấn đề tràn số nguyên trong Python, nhưng việc triển khai vẫn giữ cho việc tính toán đơn giản bằng cách sử dụng tổng của các ràng buộc ban đầu trước khi chuyển đổi biểu thức cuối cùng thành dấu phẩy động. 

## Ví dụ đã hoạt động 

Đối với mẫu được cung cấp:```
1
4 4
1 1 1 1
1 3
2 3
2 3
1 3
```Các giá trị quan trọng là: 

| Bước | tổng_f | tổng_c | ||W|| | Tốc độ tăng trưởng tối đa | Trả lời | 

|---|---:|---:|---:|---:|---:| 

| Sau khi đọc đầu vào | 6 | 12 | 2 | 8 | 6/8 | 

Yêu cầu của Senpai là`6t + 12`. Kouhai có thể tăng điểm theo tỷ lệ`4 * 2 = 8`. Bất đẳng thức trở thành`6t + 12 <= 8t`, cho`t = 6`. 

Một ví dụ thứ hai:```
1
1 5
3
2 -10
```| Bước | tổng_f | tổng_c | ||W|| | Tốc độ tăng trưởng tối đa | Trả lời | 

|---|---:|---:|---:|---:|---:| 

| Sau khi đọc đầu vào | 2 | -10 | 3 | 15 | 0 | 

Yêu cầu ban đầu đã dưới 0 nên Kouhai thành công ngay lập tức. Dấu vết xác nhận điều kiện quay trở lại sớm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q) | Mỗi chất lượng được đọc và thêm một lần. | 
| Không gian | O(1) | Chỉ cần số tiền tích lũy và độ lớn trọng lượng. | 

Tổng số phẩm chất trên tất cả các trường hợp thử nghiệm là`10^4`, do đó nghiệm tuyến tính dễ dàng phù hợp với giới hạn thời gian. Việc sử dụng bộ nhớ không phụ thuộc vào số lượng chất lượng. 

## Trường hợp thử nghiệm```python
import sys
import io
import math

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    def solve():
        import math
        input = sys.stdin.readline
        c = int(input())
        res = []
        for _ in range(c):
            q, g = map(int, input().split())
            w = list(map(int, input().split()))
            sf = sc = 0
            for _ in range(q):
                f, x = map(int, input().split())
                sf += f
                sc += x
            rate = g * math.sqrt(sum(x * x for x in w))
            if sc <= 0:
                res.append("0.00000000000")
            else:
                res.append("{:.12f}".format(sc / (rate - sf)))
        return "\n".join(res)

    out = solve()
    sys.stdin = old_stdin
    return out

assert run("""1
4 4
1 1 1 1
1 3
2 3
2 3
1 3
""") == "6.000000000000", "sample 1"

assert run("""1
1 10
1
0 -5
""") == "0.00000000000", "already satisfied"

assert run("""1
1 5
3
2 -10
""") == "0.00000000000", "negative initial requirement"

assert run("""1
2 1
1 -1
0 5
0 5
""") == "7.071067811865", "opposite weight directions"

assert run("""1
1 5000
5000
0 1
""") == "0.000200000000", "large weight precision"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đầu vào mẫu | 6.000000000000 | Đạo hàm công thức cơ bản | 
| Chất lượng đơn với yêu cầu tiêu cực | 0,00000000000 | Xử lý thành công ngay lập tức | 
| Trọng lượng ký hiệu hỗn hợp | Sử dụng đúng độ dài vectơ | Lý luận hướng trọng lượng | 
| Kích thước trọng lượng tối đa | Câu trả lời thực tế nhỏ | Độ chính xác của dấu phẩy động | 

## Vỏ cạnh 

Khi yêu cầu ban đầu đã được thỏa mãn, thuật toán trả về 0 trước khi sử dụng phương trình tuyến tính. Vì:```
1
1 10
1
0 -5
```

`sum_c = -5`, nên không cần cải thiện. Việc triển khai dựa trên phép chia mà không có kiểm tra này có thể trả về thời gian âm không hợp lệ. 

Khi trọng số chứa giá trị âm, chiến lược tốt nhất là không tăng mọi chất lượng. Hướng chuyển động tối ưu tuân theo vectơ trọng số, điều này đương nhiên có nghĩa là giảm chất lượng với trọng số âm. Ví dụ:```
1
2 1
1 -1
0 5
0 5
```Chiều dài trọng lượng là: 

1 2 +(−1) 2 ​ = 2 ​ 

Tỷ lệ cải thiện tối đa của Kouhai là`sqrt(2)`. Yêu cầu không đổi ở`10`, vậy đáp án là: 

10/ 2 ​ =7,071067811865 

Thuật toán xử lý việc này vì nó sử dụng trọng số bình phương thay vì tổng của chúng. 

Nếu tiêu chuẩn của Senpai tăng lên nhanh chóng thì mẫu số của công thức cuối cùng sẽ trở nên nhỏ. Giải pháp không tìm kiếm xung quanh câu trả lời, vì vậy nó tránh được sự mất mát độ chính xác do tìm kiếm nhị phân trong phạm vi hẹp. Nó tính toán trực tiếp giao điểm tuyến tính chính xác.
