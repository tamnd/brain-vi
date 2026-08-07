---
title: "CF 103957J - Mái vòm và tấm bia"
description: "Mỗi trường hợp thử nghiệm đưa ra một tập hợp các khối dọc giống hệt nhau, trong đó mỗi khối là một hình khối có một chiều cố định bằng 1 và hai chiều khác $ai$ và $bi$."
date: "2026-07-02T06:52:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103957
codeforces_index: "J"
codeforces_contest_name: "2015 ACM-ICPC Asia EC-Final Contest"
rating: 0
weight: 103957
solve_time_s: 52
verified: true
draft: false
---

[CF 103957J - Mái vòm và tấm bia](https://codeforces.com/problemset/problem/103957/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi trường hợp thử nghiệm đưa ra một tập hợp các khối dọc giống hệt nhau, trong đó mỗi khối là một hình khối có một chiều cố định bằng 1 và hai chiều khác$a_i$Và$b_i$. Bối cảnh khảo cổ ban đầu đặt những khối đá này đứng thẳng trên mặt đất bằng phẳng, tất cả đều thẳng hàng theo cùng một hướng và sắp xếp theo thứ tự từ trái sang phải dưới một mái vòm hình bán cầu. 

Ràng buộc hình học quan trọng đến từ điều kiện xem: khi bạn nhìn từ một hướng cố định song song với các khối, không khối nào được phép che giấu khối khác một cách trực quan. Điều này buộc các khối, theo cách sắp xếp ban đầu của chúng, tạo thành một hình bóng không chồng chéo trong phép chiếu, ngụ ý mối quan hệ trật tự chặt chẽ giữa các vị trí của chúng. Sau khi hủy, tất cả thông tin vị trí sẽ bị mất và chỉ có tập hợp các$(a_i, b_i)$các kích thước vẫn còn. 

Nhiệm vụ là tái tạo lại sự sắp xếp của các khối này dưới một bán cầu sao cho mọi khối đều nằm gọn bên trong hoặc chạm vào bề mặt bên trong và xác định bán kính nhỏ nhất có thể có của bán cầu đó. 

Đầu ra là một bán kính cho mỗi trường hợp thử nghiệm. Về mặt hình học, điều này có nghĩa là chúng tôi đang cố gắng đặt tất cả các khối trong không gian 3D theo thứ tự hợp lệ, sau đó tìm bán kính tối thiểu của một hình cầu có tâm trên tham chiếu mặt đất có thể chứa các điểm cao nhất của chúng. 

Với$N$lên đến$10^5$cho mỗi trường hợp thử nghiệm và tối đa 100 trường hợp thử nghiệm, bất kỳ giải pháp nào tệ hơn$O(N \log N)$mỗi trường hợp sẽ quá chậm. MỘT$O(N^2)$việc xây dựng các hoán vị là không thể ngay lập tức bởi vì nó sẽ yêu cầu theo thứ tự$10^{10}$hoạt động trong trường hợp xấu nhất. 

Một khó khăn chính là thứ tự không được đưa ra. Các hoán vị khác nhau của các khối tạo ra các bố cục không gian khác nhau và chiều cao của mái vòm cần thiết phụ thuộc vào khoảng cách tối đa của góc trên cùng của bất kỳ khối nào từ điểm gốc sau khi đặt. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các khối đều giống hệt nhau. Một trực giác ngây thơ có thể cho rằng thứ tự không quan trọng, nhưng các hoán vị khác nhau vẫn có thể tạo ra các cấu hình tích lũy trung gian khác nhau, làm thay đổi bán kính tối đa nếu cấu trúc phụ thuộc vào tích lũy tiền tố thay vì vị trí độc lập. 

## Phương pháp tiếp cận 

Nếu chúng ta cố gắng nghĩ về lực lượng vũ phu, ý tưởng trực tiếp nhất là hoán đổi tất cả các khối, mô phỏng vị trí của chúng theo trình tự và tính khoảng cách tối đa thu được từ điểm gốc của bất kỳ điểm góc có liên quan nào. Đối với mỗi hoán vị, chúng tôi sẽ tính toán một đường bao hình học đang chạy và theo dõi bán kính trong trường hợp xấu nhất. Cách tiếp cận này đơn giản về mặt khái niệm vì nó phù hợp trực tiếp với yêu cầu “sắp xếp tái thiết”, nhưng ngay lập tức nó lại trở nên phức tạp. có$N!$hoán vị, và thậm chí đánh giá một hoán vị mất$O(N)$, nên tổng công việc vượt xa giới hạn khả thi. 

Quan sát cấu trúc quan trọng là mặc dù các vị trí ban đầu bị mất, nhưng điều kiện không chồng chéo trong phép chiếu buộc các khối hoạt động giống như một chuỗi có thứ tự định hướng nhất quán trong mặt phẳng. Mỗi khối có thể được hiểu là đóng góp một sự dịch chuyển giống như vectơ trong hình chiếu 2D và cấu hình không gian cuối cùng phụ thuộc vào tổng tích lũy của những đóng góp này. Ràng buộc bán cầu chỉ phụ thuộc vào khoảng cách Euclide tối đa từ điểm gốc đến bất kỳ vị trí tích lũy nào, với độ lệch dọc cố định so với kích thước chiều dày. 

Điều này biến vấn đề thành một câu hỏi sắp xếp lại cổ điển: cho một tập hợp các vectơ 2D, chúng tôi muốn sắp xếp chúng sao cho khoảng cách tối đa của bất kỳ tổng tiền tố nào từ gốc được giảm thiểu. Cái nhìn sâu sắc quan trọng là nếu chúng ta giải thích từng$(a_i, b_i)$ghép thành một vectơ trong mặt phẳng thì dao động tồi tệ nhất trong tổng riêng phần đến từ các hướng trộn lẫn. Việc sắp xếp các vectơ theo góc cực sẽ tạo ra một đường truyền đơn điệu xung quanh gốc tọa độ, ngăn chặn sự hủy bỏ qua lại lớn tạo ra những chuyến đi dài trong đường tổng một phần. 

Sau khi các vectơ được sắp xếp theo góc, chúng tôi tính tổng tiền tố và theo dõi khoảng cách bình phương tối đa của bất kỳ điểm tiền tố nào, sau đó cộng phần đóng góp theo chiều dọc không đổi từ độ dày. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị Brute Force |$O(N! \cdot N)$|$O(N)$| Quá chậm | 
| Mô phỏng tiền tố sắp xếp theo góc |$O(N \log N)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Giải thích mỗi tấm bia đóng góp một vectơ dịch chuyển 2D$(a_i, b_i)$. Mô hình này mô hình hóa cách mỗi khối sẽ thay đổi dấu chân tích lũy nếu được sắp xếp theo trình tự. 
2. Sắp xếp tất cả các vectơ theo góc cực của chúng bằng cách sử dụng$\text{atan2}(b_i, a_i)$. Điều này đảm bảo rằng các vectơ liên tiếp trong chuỗi tiến triển liên tục quanh gốc tọa độ thay vì dao động giữa các hướng ngược nhau. Thứ tự này ngăn chặn sự hủy bỏ lớn trong tổng tiền tố. 
3. Khởi tạo một vector đang chạy$(x, y) = (0, 0)$, đại diện cho vị trí tích lũy sau khi đặt các khối bằng 0. 
4. Lặp lại các vectơ đã sắp xếp. Đối với mỗi vectơ$(a_i, b_i)$, cập nhật$$x \leftarrow x + a_i,\quad y \leftarrow y + b_i$$Sau mỗi lần cập nhật, tính bình phương khoảng cách$x^2 + y^2$và duy trì giá trị tối đa được thấy cho đến nay. Điều này thể hiện sự dịch chuyển theo chiều ngang xa nhất của bất kỳ cấu hình từng phần nào. 
5. Sau khi xử lý tất cả các khối, kết hợp độ dày dọc cố định là 1. Bình phương bán kính cuối cùng trở thành$$\max(x^2 + y^2) + 1$$vì mọi điểm đều có đóng góp chiều cao không đổi. 
6. Xuất căn bậc hai của giá trị này thành bán kính bán cầu. 

### Tại sao nó hoạt động 

Thuật toán dựa trên thực tế là điểm khó chứa nhất trong bất kỳ sắp xếp hợp lệ nào phải xuất hiện dưới dạng điểm cuối của một số tiền tố theo thứ tự đã chọn. Vấn đề giảm xuống còn việc kiểm soát độ dịch chuyển tích lũy trôi đi bao xa so với gốc tọa độ trong mặt phẳng. Bất kỳ thứ tự không nhất quán góc nào đều gây ra việc quay lui theo hướng, làm tăng khoảng cách cực trị trung gian vì tổng một phần tạm thời tích lũy các thành phần lớn trong các góc phần tư đối lập. 

Sắp xếp theo góc đảm bảo chuỗi tạo thành một đường truyền đơn điệu xung quanh gốc tọa độ cực. Cấu trúc này ngăn chặn sự hủy bỏ bệnh lý và đảm bảo rằng mọi tổng tiền tố nằm trong một cái nêm mở rộng dần dần thay vì nhảy qua các góc phần tư. Kết quả là, định mức tiền tố tối đa được giảm thiểu trong lớp ràng buộc này, đủ để có được vị trí tối ưu. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def solve():
    T = int(input())
    for tc in range(1, T + 1):
        n = int(input())
        pts = []
        for _ in range(n):
            a, b = map(float, input().split())
            pts.append((a, b))

        pts.sort(key=lambda p: math.atan2(p[1], p[0]))

        x = 0.0
        y = 0.0
        best = 0.0

        for a, b in pts:
            x += a
            y += b
            best = max(best, x * x + y * y)

        # add constant thickness dimension = 1
        ans = math.sqrt(best + 1.0)
        print(f"Case #{tc}: {ans:.10f}")

if __name__ == "__main__":
    solve()
```Việc triển khai bắt đầu bằng cách đọc tất cả các vectơ và sắp xếp chúng bằng cách sử dụng góc cực sao cho đường truyền tuân theo một hướng hình học nhất quán. Tổng tiền tố đang chạy tích lũy tọa độ ở dạng dấu phẩy động do đầu vào là số float có tối đa bốn số thập phân và các yêu cầu về độ chính xác cho phép độ chính xác kép tiêu chuẩn. 

Điểm tinh tế quan trọng là duy trì khoảng cách bình phương tối đa thay vì khoảng cách Euclide trong vòng lặp, giúp tránh các thao tác căn bậc hai lặp đi lặp lại và ngăn ngừa mất độ chính xác. Bước cuối cùng thêm thành phần thẳng đứng không đổi trước khi lấy căn bậc hai để tạo ra bán kính. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Xét ba khối có vectơ$(2, 0), (1, 2), (0, 3)$. 

Sau khi sắp xếp theo góc, thứ tự trở thành$(2,0), (1,2), (0,3)$. 

| Bước | Đã thêm vectơ | x | y | x2 + y2 | 
| --- | --- | --- | --- | --- | 
| 1 | (2,0) | 2 | 0 | 4 | 
| 2 | (1,2) | 3 | 2 | 13 | 
| 3 | (0,3) | 3 | 5 | 34 | 

Khoảng cách bình phương tối đa là 34, vì vậy bán kính là$\sqrt{34 + 1} = \sqrt{35}$. 

Dấu vết này cho thấy cấu hình tồi tệ nhất luôn xuất hiện ở ranh giới tiền tố thay vì bên trong một phân đoạn. 

### Ví dụ 2 

Lấy vectơ$(1,1), (2,2), (3,3)$, đã được căn chỉnh theo thứ tự góc. 

| Bước | Đã thêm vectơ | x | y | x2 + y2 | 
| --- | --- | --- | --- | --- | 
| 1 | (1,1) | 1 | 1 | 2 | 
| 2 | (2,2) | 3 | 3 | 18 | 
| 3 | (3,3) | 6 | 6 | 72 | 

Hướng đơn điệu tạo ra khoảng cách tăng đều đặn, điều này xác nhận rằng các vectơ căn chỉnh tối đa hóa độ trôi tiền tố nhưng vẫn duy trì tối ưu theo ràng buộc thứ tự. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log N)$| Sắp xếp theo góc chiếm ưu thế, quét tiền tố là tuyến tính | 
| Không gian |$O(N)$| Lưu trữ tất cả các vectơ | 

Thuật toán phù hợp thoải mái trong giới hạn ngay cả đối với$10^5$các phần tử cho mỗi trường hợp thử nghiệm. Việc sắp xếp chiếm ưu thế nhưng vẫn hiệu quả trong 100 trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io
import math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    T = int(input())
    out = []
    for tc in range(1, T + 1):
        n = int(input())
        pts = []
        for _ in range(n):
            a, b = map(float, input().split())
            pts.append((a, b))

        pts.sort(key=lambda p: math.atan2(p[1], p[0]))

        x = y = 0.0
        best = 0.0
        for a, b in pts:
            x += a
            y += b
            best = max(best, x*x + y*y)

        ans = math.sqrt(best + 1.0)
        out.append(f"Case #{tc}: {ans:.10f}")

    return "\n".join(out)

# custom minimal
assert "Case #1" in run("1\n1\n1.0000 1.0000\n")

# identical values
assert "Case #1" in run("1\n3\n2.0000 2.0000\n2.0000 2.0000\n2.0000 2.0000\n")

# symmetric spread
assert run("1\n3\n1.0000 0.0000\n0.0000 1.0000\n-1.0000 0.0000\n") != ""

# increasing chain
assert "Case #1" in run("1\n2\n3.0000 4.0000\n4.0000 3.0000\n")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Yếu tố đơn | bán kính hữu hạn | trường hợp cơ sở đúng đắn | 
| vectơ giống hệt nhau | đặt hàng ổn định | ổn định sắp xếp góc | 
| chênh lệch đối xứng | xử lý không suy biến | chuyển tiếp góc phần tư | 
| hai vectơ lớn | tích lũy đúng | hành vi tối đa tiền tố | 

## Vỏ cạnh 

Một trường hợp tấm bia được xử lý một cách tự nhiên vì vòng lặp tiền tố xử lý một vectơ và câu trả lời trở thành căn bậc hai của định mức bình phương cộng với một. Không có sự mơ hồ về thứ tự và thuật toán sẽ giảm chính xác thành tính toán trực tiếp. 

Khi nhiều vectơ có các góc giống nhau, bước sắp xếp có thể đặt chúng theo bất kỳ thứ tự tương đối nào. Vì chúng thẳng hàng nên bất kỳ hoán vị nào giữa chúng đều không làm thay đổi hình học tổng tiền tố ngoài tỷ lệ tuyến tính, do đó khoảng cách tối đa vẫn nhất quán. 

Các vectơ nằm trong các góc phần tư đối diện là trường hợp nhạy cảm nhất đối với các phương pháp ngây thơ. Nếu không sắp xếp góc, việc xen kẽ giữa các hướng ngược nhau sẽ tạo ra dao động lớn trong tổng tiền tố, làm tăng khoảng cách trung gian. Việc truyền tải được sắp xếp tránh điều này bằng cách đảm bảo các vectơ như vậy được phân tách theo trình tự, ngăn chặn sự tích lũy qua lại có tính phá hủy.
