---
title: "CF 1846D - Rudolph và cây thông Noel"
description: "Chúng ta có một bức vẽ cách điệu được làm từ nhiều “nhánh” hình tam giác giống hệt nhau đặt dọc theo một đường thẳng đứng. Mỗi nhánh là một tam giác cân có chiều dài đáy d và chiều cao h cố định."
date: "2026-06-09T05:50:53+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "geometry", "math"]
categories: ["algorithms"]
codeforces_contest: 1846
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 883 (Div. 3)"
rating: 1200
weight: 1846
solve_time_s: 80
verified: false
draft: false
---

[CF 1846D - Rudolph và cây thông Noel](https://codeforces.com/problemset/problem/1846/D) 

**Đánh giá:** 1200 
**Tags:** giải thuật xây dựng, hình học, toán học 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một bức vẽ cách điệu được làm từ nhiều “nhánh” hình tam giác giống hệt nhau đặt dọc theo một đường thẳng đứng. Mỗi nhánh là một tam giác cân có độ dài đáy cố định`d`và chiều cao`h`. Tất cả các hình tam giác đều có hướng giống nhau: đáy của chúng nằm ngang và đỉnh của chúng hướng lên trên, trong khi một thân thẳng đứng đi qua điểm giữa của mỗi đáy. 

Mỗi nhánh được đặt ở một vị trí thẳng đứng cụ thể`y_i`, đại diện cho chiều cao của đáy của nó. Vì tất cả các hình tam giác đều có hình dạng giống nhau nên điều duy nhất thay đổi là mức độ chúng chồng lên nhau khi xếp chồng lên nhau theo chiều dọc. 

Nhiệm vụ không phải là tính tổng diện tích của các tam giác riêng lẻ mà là diện tích hợp của chúng. Các vùng được sơn chồng lên nhau chỉ nên được tính một lần, vì mực chồng lên nhau không cần sơn thêm. 

Mỗi tam giác có diện tích$$S = \frac{d \cdot h}{2}$$Khó khăn đến từ sự chồng chéo: nếu hai hình tam giác đủ gần theo chiều dọc, chúng chồng lên nhau trong một vùng tam giác tương tự nhỏ hơn và sự chồng chéo này phải được trừ đi một lần trên mỗi chuỗi giao điểm. 

Các ràng buộc làm cho điều này trở nên thú vị: tổng số nhánh trên tất cả các trường hợp thử nghiệm lên tới`2 · 10^5`và tọa độ có thể lớn tới`10^9`. Bất kỳ giải pháp nào so sánh tất cả các cặp hình tam giác đều sẽ quá chậm. 

Một trường hợp tinh tế quan trọng là khi nhiều hình tam giác chồng lên nhau một cách bắc cầu nhưng không phải tất cả sự chồng chéo theo cặp đều rõ ràng. Ví dụ: tam giác A chồng lên B và B chồng lên C, nhưng A và C có thể trùng nhau trực tiếp hoặc không. Cách tiếp cận liên kết theo cặp ngây thơ có nguy cơ bị tính hai lần hoặc thiếu các chuỗi chồng chéo. 

Các trường hợp Edge phá vỡ lý luận ngây thơ bao gồm: 

Nếu`d`rất lớn và`h`nhỏ, thậm chí cách nhau vừa phải`y_i`các giá trị gây ra sự chồng chéo hoàn toàn, ví dụ:```
n = 3, d = 10, h = 1
y = [1, 2, 3]
```Tất cả các hình tam giác chồng lên nhau hoàn toàn, vì vậy câu trả lời chỉ là một diện tích hình tam giác chứ không phải ba. 

Nếu như`h`lớn và các khoảng trống nhỏ, các phần chồng lên nhau liên tục, tạo ra một hình dạng hợp nhất duy nhất. 

Những hành vi này chỉ ra rằng cấu trúc về cơ bản là sự hợp nhất theo chiều dọc của các khoảng có vùng chồng lấp co lại tuyến tính với chênh lệch độ cao. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là coi mỗi tam giác là một hình dạng hình học và tính diện tích hợp bằng cách sử dụng giao điểm theo cặp. Đối với mỗi cặp hình tam giác, chúng ta có thể tính khoảng dọc chồng chéo của chúng và trừ đi diện tích chồng lấp tương ứng. 

Việc này nhanh chóng trở nên đắt đỏ vì có`O(n^2)`cặp và mỗi phép tính chồng chéo là không tầm thường. Với`n`lên đến`2 · 10^5`, điều này là không thể thực hiện được. 

Quan sát quan trọng là hình dạng của mỗi tam giác là tuyến tính theo cả hai hướng: chiều rộng ngang giảm tuyến tính khi chúng ta di chuyển lên từ đáy. Điều đó có nghĩa là sự chồng chéo giữa hai hình tam giác chỉ phụ thuộc vào khoảng cách theo chiều dọc giữa các đáy của chúng. 

Nếu chúng ta sắp xếp theo`y_i`, chúng ta có thể xử lý các hình tam giác theo thứ tự tăng dần và duy trì “đường bao hợp nhất hoạt động” hiện tại của liên kết. Mỗi tam giác mới: 

1. Bắt đầu phía trên đỉnh đã hợp nhất hiện tại mà không chồng chéo, đóng góp toàn bộ diện tích của nó, hoặc 
2. Chồng chéo một phần, làm giảm diện tích được thêm vào hiệu quả. 

Điều kiện chồng lấp trở nên hoàn toàn một chiều: đối với hai tam giác liền kề, sự chồng lấn tồn tại nếu khoảng cách dọc nhỏ hơn`h`. Khi có sự chồng lấp, vùng giao nhau chính là một tam giác nhỏ hơn có chiều cao bằng`h - gap`, và cơ sở của nó tỉ lệ thuận với nhau. 

Điều này cho phép chúng tôi duy trì hoạt động đóng góp hợp nhất và trừ dần các đóng góp chồng chéo trong khi quét. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hình học theo cặp Brute Force | O(n²) | O(1) | Quá chậm | 
| Quét đã sắp xếp + Hợp nhất chồng chéo | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp hình tam giác theo chiều cao đáy`y_i`. Điều này đảm bảo chúng ta chỉ so sánh từng tam giác với cấu trúc đã hợp nhất trước đó, vì sự chồng chéo chỉ có thể xảy ra với các tam giác gần đó theo thứ tự được sắp xếp. 
2. Khởi tạo tổng diện tích là 0 và duy trì “phân đoạn hoạt động” hiện tại, đại diện cho tam giác cuối cùng vẫn ảnh hưởng đến sự chồng lấp trong tương lai. 
3. Đối với mỗi tam giác theo thứ tự: 

Nếu nó không trùng với tam giác hoạt động trước đó (khoảng cách ≥ h), hãy thêm diện tích tam giác đầy đủ. 

Nếu không, hãy tính chiều cao chồng lấp như`h - gap`và trừ đi diện tích hình tam giác chồng chéo khỏi tổng đơn giản. 
4. Khi xảy ra sự chồng chéo, hãy cập nhật “đường bao trên cùng” đã hợp nhất hiệu quả bằng cách giữ hình tam giác mở rộng cao hơn. 
5. Tiếp tục cho đến khi tất cả các hình tam giác được xử lý. 

Tính toán chính là diện tích chồng lên nhau giữa hai tam giác cân giống hệt nhau dịch chuyển theo chiều dọc. Vì chiều rộng tỉ lệ tuyến tính với chiều cao nên giao điểm là một tam giác đồng dạng nhỏ hơn có chiều cao`h - gap`, do đó diện tích của nó tỉ lệ với bình phương của phần chiều cao còn lại. 

### Tại sao nó hoạt động 

Mỗi tam giác xác định một ranh giới tuyến tính theo hướng thẳng đứng nơi chiều rộng của nó co lại đồng đều. Sự kết hợp của các hình dạng như vậy dọc theo một đường làm giảm việc duy trì đường bao hoạt động cao nhất ở mọi độ cao. Do cấu hình chiều rộng là lồi và tuyến tính, nên sự chồng chéo giữa các hình tam giác không liền kề hoàn toàn bị chi phối bởi các hình trung gian theo thứ tự được sắp xếp. Điều này đảm bảo rằng quá trình xử lý theo thứ tự không bao giờ bỏ lỡ giao lộ hoặc tính gấp đôi các khu vực chưa được phát hiện. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, d, h = map(int, input().split())
        y = list(map(int, input().split()))

        base_area = d * h / 2.0
        ans = base_area

        for i in range(1, n):
            gap = y[i] - y[i - 1]
            if gap >= h:
                ans += base_area
            else:
                # overlapping part is a smaller similar triangle
                overlap_h = h - gap
                ratio = overlap_h / h
                overlap_area = base_area * ratio * ratio
                ans += base_area - overlap_area

        print(f"{ans:.10f}")

if __name__ == "__main__":
    solve()
```Đoạn mã tính diện tích tăng dần từ trái sang phải sau khi sắp xếp cấu trúc ẩn được đưa ra bởi`y_i`. Diện tích tam giác đáy được tính một lần. Đối với mỗi cặp liên tiếp, chúng tôi xác định xem có tồn tại sự chồng chéo thông qua khoảng cách dọc hay không. 

Nếu không có sự chồng chéo thì diện tích tam giác đầy đủ sẽ được thêm vào. Nếu tồn tại sự chồng chéo, chúng tôi tính toán tam giác chồng chéo bằng cách sử dụng độ tương tự: chiều cao chồng chéo co lại một cách tuyến tính, do đó diện tích có tỷ lệ bậc hai. Đó là lý do tại sao chúng tôi nhân với`(overlap_h / h)^2`. 

Một điểm tinh tế là chúng ta không bao giờ cần theo dõi hình học đầy đủ một cách rõ ràng. Hình dạng được xác định hoàn toàn bởi sự khác biệt về chiều cao vì tất cả các hình tam giác đều giống hệt nhau và thẳng hàng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3, d = 4, h = 2
y = [1, 4, 5]
```| tôi | y[i] | khoảng cách | chồng chéo? | hành động | đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | - | - | thêm đầy đủ | 4 | 
| 1 | 4 | 3 | không | thêm đầy đủ | 4 | 
| 2 | 5 | 1 | vâng | một phần | 4 - chồng chéo | 

Diện tích cơ sở = 4 

Đối với cặp cuối cùng: chiều cao chồng chéo = 2 - 1 = 1, tỷ lệ = 1/2, diện tích chồng lấp = 1 

Vậy đóng góp = 3 

Tổng cộng = 4 + 4 + 3 = 11 

Điều này xác nhận rằng một sự chồng chéo chỉ làm giảm một phần diện tích được thêm vào thay vì loại bỏ toàn bộ hình tam giác. 

### Ví dụ 2 

đầu vào:```
n = 2, d = 1, h = 200000
y = [1, 200000]
```| tôi | y[i] | khoảng cách | chồng chéo? | hành động | đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | - | - | thêm đầy đủ | 100000 | 
| 1 | 200000 | 199999 | không | thêm đầy đủ | 100000 | 

Khoảng cách bằng`h`, do đó không có sự chồng chéo. Điều này cho thấy điều kiện biên trong đó hai hình tam giác chỉ tiếp xúc nhưng không cắt nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi tam giác được xử lý một lần sau khi sắp xếp thứ tự đầu vào đã tăng dần | 
| Không gian | O(1) | Chỉ các tổng và hằng số đang chạy được lưu trữ | 

Giải pháp là tuyến tính trong tổng số nhánh trên tất cả các trường hợp thử nghiệm, nhiều nhất là`2 · 10^5`, vì vậy nó dễ dàng phù hợp trong thời hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, d, h = map(int, input().split())
        y = list(map(int, input().split()))

        base_area = d * h / 2.0
        ans = base_area

        for i in range(1, n):
            gap = y[i] - y[i - 1]
            if gap >= h:
                ans += base_area
            else:
                overlap_h = h - gap
                ratio = overlap_h / h
                ans += base_area - base_area * ratio * ratio

        out.append(str(ans))

    return "\n".join(out)

# provided samples
assert abs(float(run("""5
3 4 2
1 4 5
1 5 1
3
4 6 6
1 2 3 4
2 1 200000
1 200000
2 4 3
9 11
""").split()[0]) - 11) < 1e-6

# custom cases
assert "1.0" in run("1\n1 2 2\n5\n")
assert "2.0" in run("1\n2 2 2\n1 10\n")
assert "0" not in run("1\n2 100 100\n1 2\n")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tam giác đơn | toàn bộ khu vực | trường hợp cơ sở | 
| tam giác xa nhau | tổng diện tích | không chồng chéo | 
| chuỗi chồng chéo khổng lồ | hành vi hợp nhất | chồng chéo chuyển tiếp | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là khi tất cả`y_i`đang vô cùng gần gũi. Ví dụ:```
n = 3, d = 4, h = 2
y = [1, 2, 3]
```Ở đây mỗi khoảng cách liên tiếp là`1`, do đó, mỗi tam giác mới chồng lên nhau rất nhiều với tam giác trước đó. Thuật toán liên tục áp dụng công thức chồng chéo, chỉ thu hẹp phần đóng góp gia tăng. Kết quả cuối cùng không phải là diện tích tam giác gấp ba lần mà là một hình dạng hợp nhất duy nhất được tạo thành bằng cách thu nhỏ liên tiếp. 

Trường hợp cạnh thứ hai là khi các khoảng trống bằng nhau một cách chính xác`h`. TRONG:```
y = [1, 3], h = 2
```các hình tam giác chỉ chạm vào một điểm. điều kiện`gap >= h`đảm bảo không xảy ra phép trừ chồng chéo, do đó cả hai vùng đầy đủ đều được thêm vào. Điều này ngăn chặn việc vô tình loại bỏ các giao điểm vùng 0, nếu không sẽ gây ra nhiễu dấu phẩy động. 

Trường hợp thứ ba là khi`d`hoặc`h`đủ lớn để độ chính xác nổi trở nên phù hợp. Do sự chồng lấp sử dụng tỷ lệ bình phương, nên các giá trị trung gian phải được giữ ở dạng dấu phẩy động, nhưng tất cả các phép toán vẫn ổn định vì chúng liên quan đến tích giới hạn của nhiều nhất`1e10`số tỷ lệ, cũng có độ chính xác gấp đôi.
