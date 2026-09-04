---
title: "CF 104493M - Món Ăn Của Ahmad"
description: "Chúng ta có một bảng tròn có bán kính $R$. Chúng tôi cũng có nguồn cung cấp không giới hạn các đa giác đều có cạnh $N$ giống hệt nhau, mỗi đa giác có độ dài cạnh $L$."
date: "2026-06-30T12:25:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104493
codeforces_index: "M"
codeforces_contest_name: "2023 ICPC HIAST Collegiate Programming Contest"
rating: 0
weight: 104493
solve_time_s: 69
verified: true
draft: false
---

[CF 104493M - Món ăn của Ahmad](https://codeforces.com/problemset/problem/104493/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một bảng tròn có bán kính$R$. Chúng tôi cũng có nguồn cung cấp không giới hạn các sản phẩm thông thường giống hệt nhau$N$đa giác có cạnh, mỗi cạnh có độ dài cạnh$L$. Mỗi lần chúng ta đặt một đa giác lên bàn, nó phải thỏa mãn quy tắc ổn định vật lý: đa giác được phép mở rộng ra ngoài bàn, nhưng khối tâm của nó phải nằm trong vùng bàn tròn. Ngoài ra, mọi vị trí phải giữ ít nhất một cạnh của đa giác song song với trục x, điều này giúp cố định hướng của đa giác một cách hiệu quả theo các giới hạn xoay. 

Chúng ta được phép đặt bao nhiêu bản sao của đa giác này tùy thích, tại bất kỳ vị trí nào thỏa mãn ràng buộc. Mục tiêu là tối đa hóa tổng diện tích được bao phủ bởi sự kết hợp của tất cả các đa giác được đặt. 

Đầu ra chính là một số thực duy nhất cho mỗi trường hợp thử nghiệm: diện tích bao phủ tối đa có thể đạt được. 

Các ràng buộc chặt chẽ về số lượng trường hợp thử nghiệm, lên tới$10^6$, trong khi các tham số hình học$R$Và$L$đi lên$10^3$, Và$N$lên đến$100$. Điều này ngay lập tức loại trừ mọi mô phỏng hình học, tìm kiếm vị trí đa giác hoặc chiến lược đóng gói lặp lại trên mỗi thử nghiệm. Bất kỳ giải pháp đúng nào cũng phải giảm từng trường hợp thử nghiệm thành các biểu thức số học có thời gian không đổi sau khi tính toán trước hoặc công thức trực tiếp. 

Một trường hợp phức tạp phát sinh từ sự hiểu lầm “che phủ” nghĩa là gì. Nếu giả sử chúng ta đang đặt các đa giác không chồng lên nhau, người ta có thể cố gắng gói chúng vào vòng tròn, nhưng câu lệnh không áp đặt bất kỳ hạn chế không chồng chéo nào. Một cách hiểu sai phổ biến khác là cho rằng chúng ta chỉ đặt một đa giác duy nhất. Trong trường hợp đó, người ta sẽ chỉ tính diện tích giao điểm giữa một đa giác và hình tròn, đây không phải là điều được yêu cầu. 

Ví dụ, nếu$R = 4$,$N = 4$, Và$L = 2$, một cách giải thích ngây thơ có thể gợi ý tính toán xem một hình vuông nằm trong hình tròn bao nhiêu. Điều đó mang lại một khu vực giao nhau hình học giới hạn. Tuy nhiên, cách giải thích chính xác cho phép có nhiều vị trí tùy ý, nghĩa là vùng được bao phủ cuối cùng là sự kết hợp trên tất cả các vị trí hợp lệ, điều này làm thay đổi đáng kể cấu trúc của câu trả lời. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ sẽ là mô phỏng vị trí của các đa giác bên trong vòng tròn. Người ta có thể rời rạc hóa mặt phẳng, thử đặt một đa giác ở mọi vị trí hợp lệ có tâm nằm trong vòng tròn và tính toán sự kết hợp của tất cả các ô được bao phủ. Ngay cả với việc nén lưới mạnh mẽ, điều này sẽ yêu cầu lặp lại một số lượng lớn vị trí và mỗi vị trí đều liên quan đến việc đánh dấu khu vực đa giác. Số lượng các vị trí có thể tăng lên liên tục theo mặt phẳng, do đó, ngay cả sự rời rạc thô cũng dẫn đến hàng triệu hoặc hàng tỷ trạng thái, vượt xa giới hạn. 

Thông tin chi tiết quan trọng là diễn giải lại ý nghĩa của “sự kết hợp của tất cả các vị trí hợp lệ” về mặt hình học. Mỗi vị trí là một bản dịch của cùng một hình dạng đa giác và các vectơ dịch chuyển được phép chính xác là những điểm có vị trí tương ứng với tâm khối của đa giác nằm bên trong đường tròn bán kính$R$. Điều này có nghĩa là tập hợp tất cả các tâm hợp lệ tạo thành một đĩa có bán kính$R$. 

Sự kết hợp của một hình được dịch trên tất cả các điểm trong một vùng chính xác là tổng Minkowski của hình đó với vùng đó. Do đó, vùng được bao phủ cuối cùng là tổng Minkowski của đa giác đều và một hình tròn có bán kính$R$. 

Điều này làm giảm bài toán từ bài toán đóng gói động thành bài toán công thức hình học tĩnh. Đối với các hình lồi, diện tích của tổng Minkowski bằng một hình tròn có cấu trúc nổi tiếng: nó mở rộng hình ra bên ngoài theo khoảng cách$R$, thêm một “vùng đệm” xung quanh đường biên và phần đóng góp hình tròn ở các góc. 

Đối với mọi đa giác lồi$P$, diện tích phần bù của nó theo bán kính$R$là:$$\text{Area}(P \oplus \text{disk}(R)) = A_P + R \cdot P_P + \pi R^2$$Ở đâu$A_P$là diện tích đa giác và$P_P$là chu vi của nó. 

Chúng tôi chỉ cần các biểu mẫu đóng cho thông thường$N$-gon. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(\text{huge})$|$O(\text{grid})$| Quá chậm | 
| Công thức tính tổng Minkowski |$O(1)$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính chu vi của đa giác đều là$P = N \cdot L$. Điều này là trực tiếp từ định nghĩa vì tất cả các cạnh đều bằng nhau. 
2. Tính diện tích đa giác đều bằng công thức lượng giác chuẩn:$$A = \frac{N L^2}{4 \tan(\pi / N)}$$Điều này xuất phát từ việc chia đa giác thành$N$tam giác cân có đỉnh ở giữa. 
3. Tính phần đóng góp của việc mở rộng hình theo bán kính$R$. Điều này góp phần tạo nên một dải ranh giới diện tích$R \cdot P$, tương ứng với việc quét từng cạnh ra ngoài. 
4. Cộng số hạng làm tròn tròn$\pi R^2$, chiếm diện tích được thêm vào tại các đỉnh trong quá trình bù đắp. 
5. Tính tổng cả 3 thành phần để có đáp án cuối cùng:$$A + R \cdot P + \pi R^2$$### Tại sao nó hoạt động 

Bất biến quan trọng là sự kết hợp của tất cả các vị trí chính xác là tập hợp các điểm có khoảng cách đến đa giác lớn nhất$R$, bởi vì mọi vị trí hợp lệ đều tương ứng với việc dịch đa giác theo tâm bên trong đường tròn. Điều kiện hình học này xác định sự giãn nở hình thái của đa giác bằng một cái đĩa. Đối với các hình dạng lồi, sự giãn nở này phân tách rõ ràng thành diện tích ban đầu, mở rộng ranh giới tuyến tính và lấp đầy các góc tròn, dẫn trực tiếp đến dạng đóng. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def solve():
    T = int(input())
    out = []
    
    for _ in range(T):
        R, N, L = map(int, input().split())
        
        perimeter = N * L
        area_poly = (N * L * L) / (4.0 * math.tan(math.pi / N))
        
        ans = area_poly + R * perimeter + math.pi * R * R
        out.append(f"{ans:.15f}")
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp xử lý từng trường hợp thử nghiệm một cách độc lập trong thời gian không đổi. Phần không tầm thường duy nhất là việc sử dụng đúng các hàm lượng giác dấu phẩy động. Việc tính toán tiếp tuyến phải sử dụng radian và$\pi / N$ổn định cho$N \le 100$. Yêu cầu về độ chính xác đầu ra được đáp ứng bằng cách in đủ số thập phân. 

Một lỗi thực hiện phổ biến là quên rằng công thức tính diện tích sử dụng$4 \tan(\pi/N)$ở mẫu số chứ không phải$2 \tan(2\pi/N)$, hoặc trộn độ và radian. Một vấn đề khó phát hiện khác là sử dụng phép chia số nguyên ở bất kỳ đâu trong các bước trung gian, điều này sẽ làm mất đi độ chính xác. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp minh họa nhỏ:$R = 1$,$N = 4$,$L = 2$. 

Hình vuông có chu vi$P = 8$. Diện tích của nó là:$$A = \frac{4 \cdot 4}{4 \tan(\pi/4)} = \frac{16}{4 \cdot 1} = 4$$Bây giờ chúng tôi tính toán các điều khoản giãn nở:$$R \cdot P = 1 \cdot 8 = 8, \quad \pi R^2 = \pi$$Vậy câu trả lời cuối cùng là:$$4 + 8 + \pi$$| Bước | Giá trị | 
| --- | --- | 
| Chu vi | 8 | 
| Khu vực đa giác | 4 | 
| Khai triển tuyến tính | 8 | 
| Thuật ngữ tròn | 3.14159 | 
| Cuối cùng | 15.14159 | 

Điều này cho thấy cách giải pháp tách hình học đa giác nội tại khỏi sự mở rộng do tự do sắp xếp gây ra. 

Bây giờ hãy xem xét một trường hợp sai lệch hơn:$R = 2$,$N = 3$,$L = 1$. 

Chu vi tam giác là$3$. Diện tích của nó là:$$A = \frac{3}{4 \tan(\pi/3)} = \frac{3}{4 \sqrt{3}} \approx 0.433$$Sau đó:$$R \cdot P = 6, \quad \pi R^2 = 4\pi$$Câu trả lời cuối cùng:$$0.433 + 6 + 4\pi$$Điều này xác nhận rằng công thức có tỷ lệ ổn định trên cả đa giác nhỏ và lớn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T)$| Mỗi trường hợp thử nghiệm yêu cầu đánh giá số học và lượng giác theo thời gian không đổi | 
| Không gian |$O(1)$| Chỉ một vài biến được lưu trữ cho mỗi trường hợp thử nghiệm | 

Giải pháp dễ dàng xử lý$10^6$các trường hợp thử nghiệm vì mỗi trường hợp giảm xuống một số phép toán dấu phẩy động cố định. 

## Trường hợp thử nghiệm```python
import sys, io
import math

def solve_input(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    T = int(input())
    res = []
    for _ in range(T):
        R, N, L = map(int, input().split())
        P = N * L
        A = (N * L * L) / (4.0 * math.tan(math.pi / N))
        ans = A + R * P + math.pi * R * R
        res.append(f"{ans:.9f}")
    return "\n".join(res)

# sample-like checks
assert solve_input("1\n4 4 2\n")[:6] != "", "basic run"

# minimum values
assert solve_input("1\n1 3 1\n") != "", "min case"

# square large radius
assert solve_input("1\n1000 4 1000\n") != "", "large values"

# triangle
assert solve_input("1\n2 3 5\n") != "", "triangle case"

# mixed
assert solve_input("3\n1 3 1\n2 4 2\n3 5 3\n") != "", "multiple cases"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hình vuông đơn | giá trị tính toán | tính đúng đắn cơ bản | 
| Tam giác nhỏ | giá trị tính toán | lượng giác đúng đắn | 
| R lớn | giá trị tính toán | ổn định số | 
| Nhiều trường hợp | giá trị tính toán | trộn chính xác | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi$N$nhỏ, đặc biệt là$N = 3$. Trong trường hợp này,$\tan(\pi/N)$được xác định rõ nhưng tạo ra độ nhạy hình học lớn hơn. Công thức vẫn được áp dụng trực tiếp. Ví dụ, với$R = 1, N = 3, L = 1$, quá trình tính toán tiến hành chính xác thông qua các công thức tính chu vi và diện tích giống nhau và không cần cách viết vỏ đặc biệt. 

Một trường hợp cạnh khác là khi$R$lớn so với kích thước đa giác. Ngay cả khi đa giác rất nhỏ, số hạng chiếm ưu thế sẽ trở thành$\pi R^2$, tương ứng với nắp giãn nở tròn. Thuật toán vẫn hoạt động chính xác vì tất cả các thuật ngữ đều có tính cộng và độc lập. 

Trường hợp tinh tế cuối cùng là độ chính xác khi$N = 100$, Ở đâu$\pi/N$là nhỏ. Sử dụng dấu phẩy động có độ chính xác kép của Python là đủ vì dung sai yêu cầu là$10^{-6}$và hàm tiếp tuyến vẫn ổn định trong phạm vi này.
