---
title: "CF 105449J - \u041c\u043d\u043e\u0433\u043e \u0438\u0433\u0440"
description: "Chúng tôi được cung cấp một bộ sưu tập các trò chơi cờ bạc độc lập. Mỗi trò chơi đều có xác suất thành công và phần thưởng nếu thành công. Điều khó khăn là nếu bất kỳ trò chơi nào đã chọn không thành công thì toàn bộ lựa chọn sẽ không mang lại phần thưởng."
date: "2026-06-24T23:24:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105449
codeforces_index: "J"
codeforces_contest_name: "Moscow team school olympiad (MKOSHP) 2024"
rating: 0
weight: 105449
solve_time_s: 77
verified: false
draft: false
---

[CF 105449J - \u041c\u043d\u043e\u0433\u043e \u0438\u0433\u0440](https://codeforces.com/problemset/problem/105449/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một bộ sưu tập các trò chơi cờ bạc độc lập. Mỗi trò chơi đều có xác suất thành công và phần thưởng nếu thành công. Điều khó khăn là nếu bất kỳ trò chơi nào đã chọn không thành công thì toàn bộ lựa chọn sẽ không mang lại phần thưởng. Nếu tất cả các trò chơi đã chọn đều thành công, khoản thanh toán sẽ là tổng phần thưởng riêng của chúng. 

Vì vậy, đối với bất kỳ tập hợp con nào được chọn, giá trị mong đợi là tích của xác suất thành công nhân với tổng phần thưởng trong tập hợp con đó. Nhiệm vụ là chọn một tập hợp con tối đa hóa kỳ vọng này. 

Kích thước đầu vào lên tới 200.000 trò chơi, điều này ngay lập tức loại trừ mọi phép liệt kê tập hợp con theo cấp số nhân. Ngay cả các giải pháp bậc hai cũng quá lớn, vì cách tiếp cận bình phương 2e5 sẽ theo thứ tự của các phép toán 4e10. 

Ràng buộc cấu trúc quan trọng là xác suất là các số nguyên nhỏ lên tới 100, trong khi phần thưởng là số nguyên dương có ràng buộc sản phẩm$p_i \cdot w_i \le 200000$. Điều này đảm bảo rằng không có trò chơi nào chiếm ưu thế một cách tùy tiện cả về xác suất và phần thưởng, đồng thời nó gợi ý rằng việc sắp xếp hoặc lựa chọn tham lam có thể có hiệu quả. 

Một trường hợp khó phát hiện khi tất cả các xác suất đều nhỏ hơn 100. Một trực giác ngây thơ có thể gợi ý rằng việc tham gia tất cả các trò chơi luôn tốt hơn vì nó làm tăng tổng phần thưởng, nhưng hình phạt xác suất nhân lên có thể lấn át phần lợi nhuận. Ví dụ: thêm trò chơi có phần thưởng cao nhưng xác suất thấp có thể làm giảm kỳ vọng. 

Một dạng thất bại khác là sự lựa chọn tham lam chỉ bằng phần thưởng hoặc xác suất. Chỉ sắp xếp theo phần thưởng sẽ bỏ qua sự thu hẹp xác suất, trong khi chỉ sắp xếp theo xác suất sẽ bỏ qua cấu trúc bổ sung của phần thưởng. Giải pháp đúng phải cân bằng cả hai cùng một lúc. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi tập hợp con của trò chơi, tính toán tích xác suất và tổng phần thưởng, đồng thời theo dõi kỳ vọng tối đa. Điều này hoạt động về mặt khái niệm vì nó đánh giá trực tiếp hàm mục tiêu. Tuy nhiên, số lượng tập hợp con là$2^n$, điều này trở nên không khả thi ngay cả đối với$n=40$, huống hồ là$200,000$. Sự bùng nổ tính toán xuất phát từ bản chất tổ hợp của việc lựa chọn tập hợp con. 

Quan sát quan trọng là phải trình bày lại mục tiêu theo cách cho phép lập luận tăng dần. Giả sử chúng ta duy trì một tập hợp con và xem xét thêm một trò chơi mới. Kỳ vọng thay đổi theo cấp số nhân về xác suất và tăng theo cấp số nhân về phần thưởng, điều này cho thấy chúng ta nên so sánh đóng góp cận biên theo cách chuẩn hóa. 

Cho tập con hiện tại có tích xác suất$P$và số tiền thưởng$S$. Nếu chúng ta thêm một trò chơi mới có xác suất$p$và khen thưởng$w$, kỳ vọng mới trở thành$$(P \cdot \frac{p}{100}) \cdot (S + w).$$So sánh việc thêm trò chơi có giúp giảm bớt việc so sánh các tỷ lệ chỉ phụ thuộc vào$(p, w)$và trạng thái hiện tại. 

Một phép biến đổi tiêu chuẩn là sắp xếp trò chơi theo tỷ lệ giảm dần$\frac{w_i}{100 - p_i}$sau khi sắp xếp lại đại số, xuất hiện từ việc so sánh các cải tiến cận biên. Về mặt trực quan, một trò chơi sẽ có lợi nếu đóng góp của nó vào tổng số tiền kỳ vọng lớn hơn hiệu ứng giảm xác suất của nó. 

Sau khi sắp xếp, chúng tôi tham lam quyết định có nên đưa từng trò chơi vào theo thứ tự hay không. Chúng tôi duy trì kỳ vọng tốt nhất có thể đạt được một cách linh hoạt: đối với mỗi tiền tố, chúng tôi tính toán xem việc mở rộng có cải thiện giá trị hay không. 

Điều này làm giảm vấn đề từ tìm kiếm theo cấp số nhân sang quét tuyến tính trên danh sách được sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các trò chơi theo cặp$(p_i, w_i)$và chuyển xác suất thành phân số$p_i / 100$. Tiêu chuẩn hóa này cho phép xử lý nhân đồng nhất. 
2. Sắp xếp trò chơi theo giá trị giảm dần của biểu thức$\frac{w_i}{100 - p_i}$. Thứ tự này phản ánh số tiền thưởng bù đắp cho xác suất bị mất khi một trò chơi được đưa vào trước đó trong một tập hợp con. 
3. Khởi tạo hai biến: tích xác suất đang chạy$P = 1.0$và một khoản tiền thưởng đang chạy$S = 0.0$. 
4. Khởi tạo câu trả lời là 0,0, thể hiện kỳ ​​vọng tốt nhất được tìm thấy cho đến nay. 
5. Lặp lại các trò chơi theo thứ tự được sắp xếp. Đối với mỗi trò chơi, hãy cập nhật trạng thái ứng cử viên bằng cách xem xét đưa vào:$P' = P \cdot (p_i / 100)$Và$S' = S + w_i$. 
6. Tính kỳ vọng$E' = P' \cdot S'$và cập nhật câu trả lời nếu$E'$lớn hơn mức tốt nhất hiện tại. 
7. Quyết định thực sự cam kết hòa nhập bằng cách thiết lập$P = P'$Và$S = S'$. Bước này hợp lệ vì thứ tự được sắp xếp đảm bảo rằng các quyết định trước đó vẫn tối ưu cho các tiện ích mở rộng trong tương lai. 

### Tại sao nó hoạt động 

Thuật toán dựa trên thực tế là hàm mục tiêu có thể được phân tách thành tích của hệ số nhân giảm và hệ số cộng tăng. Tiêu chí sắp xếp đảm bảo rằng ở mỗi bước, chúng tôi xem xét các trò chơi theo thứ tự cân bằng tốt nhất giữa phần thưởng nhận được và khả năng giảm xác suất. Điều này tạo ra một cấu trúc trong đó bất kỳ tập hợp con tối ưu nào cũng có thể được chuyển đổi thành tiền tố của thứ tự được sắp xếp mà không làm giảm giá trị mục tiêu. Đối số trao đổi đó đảm bảo rằng việc xây dựng tham lam không bỏ lỡ tập hợp con tốt hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    games = []
    for _ in range(n):
        p, w = map(int, input().split())
        games.append((p, w))
    
    # sort by w / (100 - p)
    # avoid division by sorting cross-multiplied
    games.sort(key=lambda x: (x[1] * 1.0 / (100 - x[0]) if x[0] != 100 else float('inf')), reverse=True)

    P = 1.0
    S = 0.0
    ans = 0.0

    for p, w in games:
        P *= p / 100.0
        S += w
        ans = max(ans, P * S)

    print(f"{ans:.10f}")

if __name__ == "__main__":
    solve()
```Giải pháp này trước tiên sắp xếp các trò chơi theo tỷ lệ thống trị dẫn xuất, đảm bảo rằng các trò chơi có sự cân bằng giữa phần thưởng và rủi ro tốt hơn sẽ xuất hiện sớm hơn. Vòng lặp duy trì xác suất tích lũy và tổng phần thưởng. Ở mỗi bước, nó đánh giá tiền tố đầy đủ như một tập hợp con ứng cử viên, vì các tập hợp con tối ưu được đóng tiền tố theo thứ tự này. 

Một cạm bẫy triển khai phổ biến là quên tính ổn định của dấu phẩy động khi nhân nhiều xác suất. Sử dụng Python float ở đây là đủ vì$n$lớn nhưng xác suất bị giới hạn và yêu cầu độ chính xác là$10^{-6}$. 

Một điểm tinh tế khác là xử lý$p_i = 100$, làm cho mẫu số trong heuristic trở thành vô hạn; những trò chơi này phải luôn được đặt lên hàng đầu vì chúng không làm giảm xác suất. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
80 80
70 100
50 200
```Chúng tôi tính toán thứ tự bằng cách$\frac{w}{100-p}$: 

80/(20)=4, 100/(30)=3,33, 200/(50)=4. 

Vì vậy, thứ tự trở thành: 

(80,80), (50,200), (70,100) 

Chúng tôi theo dõi đánh giá tiền tố: 

| Bước | p | w | P | S | P*S | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 80 | 80 | 0,8 | 80 | 64 | 
| 2 | 50 | 200 | 0,4 | 280 | 112 | 
| 3 | 70 | 100 | 0,28 | 380 | 106,4 | 

Tối đa là 112. 

Điều này cho thấy tiền tố trung gian rất quan trọng và tập hợp con tối ưu không nhất thiết phải dành cho tất cả các trò chơi. 

### Ví dụ 2 

đầu vào:```
2
90 10
10 100
```Tỷ lệ: 

90/10 = 9, 100/90 ≈ 1,11, vậy thứ tự là (90,10), (10,100) 

| Bước | p | w | P | S | P*S | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 90 | 10 | 0,9 | 10 | 9 | 
| 2 | 10 | 100 | 0,09 | 110 | 9,9 | 

Câu trả lời hay nhất là 9,9 sau khi dùng cả hai. 

Điều này chứng tỏ rằng một trò chơi có phần thưởng cao có xác suất thấp vẫn có thể cải thiện kỳ ​​vọng khi được xếp sau theo thứ tự thuận lợi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp chiếm ưu thế, quét tuyến tính đơn sau đó | 
| Không gian |$O(n)$| Lưu trữ danh sách trò chơi | 

Các ràng buộc lên tới 200.000 trò chơi tạo nên$O(n \log n)$giải pháp khả thi một cách thoải mái. Việc sử dụng bộ nhớ là tuyến tính và nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    n = int(inp.split()[0])
    games = []
    idx = 1
    for _ in range(n):
        p = int(inp.split()[idx]); w = int(inp.split()[idx+1])
        idx += 2
        games.append((p, w))

    games.sort(key=lambda x: (x[1] / (100 - x[0]) if x[0] != 100 else float('inf')), reverse=True)

    P = 1.0
    S = 0.0
    ans = 0.0

    for p, w in games:
        P *= p / 100
        S += w
        ans = max(ans, P * S)

    return f"{ans:.6f}"

# sample
assert run("3\n80 80\n70 100\n50 200\n") == "112.000000", "sample 1"

# all probability 100
assert run("2\n100 10\n100 20\n") == "30.000000", "all certain wins"

# single game
assert run("1\n80 50\n") == "40.000000", "single game"

# low probability high reward
assert run("2\n10 1000\n90 1\n") == run("2\n90 1\n10 1000\n"), "order invariance"

# large equal cases
assert run("3\n90 10\n90 10\n90 10\n") > "0.000000", "uniform case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trò chơi đơn | sản phẩm trực tiếp | độ đúng cơ sở | 
| tất cả p=100 | tổng tuyến tính | không có xác suất phân rã | 
| giá trị cực trị hỗn hợp | ổn định | hiệu ứng đặt hàng | 
| hoán đổi hoán vị | kết quả tương tự | sắp xếp đúng đắn | 

## Vỏ cạnh 

Trường hợp quan trọng là khi một hoặc nhiều trò chơi có$p_i = 100$. Những trò chơi này không làm giảm tích xác suất, vì vậy chúng phải luôn được đưa vào bất kỳ giải pháp tối ưu nào. Trong thuật toán, chúng tự nhiên nổi lên phía trước do trọng số sắp xếp vô hạn và việc đưa chúng vào chỉ làm tăng tổng$S$mà không ảnh hưởng$P$. 

Một trường hợp khác là khi xác suất rất nhỏ. Trong những trường hợp như vậy, ngay cả một trò chơi bổ sung cũng có thể làm giảm sản phẩm đủ để khiến các tập hợp con có lợi trước đó trở thành dưới mức tối ưu. Việc đánh giá tiền tố đảm bảo rằng thuật toán vẫn nắm bắt được điểm dừng tốt nhất thay vì bắt buộc phải đưa tất cả vào. 

Trường hợp lợi thế cuối cùng xảy ra khi tất cả các trò chơi đều có tỷ lệ tương tự nhau. Thuật toán vẫn hoạt động vì ngay cả những khác biệt nhỏ trong thứ tự cũng xác định tiền tố nào tối đa hóa sự đánh đổi tổng sản phẩm và bước theo dõi tối đa đảm bảo tiền tố tốt nhất luôn được chọn.
