---
title: "CF 104668J - Ma trận"
description: "Chúng ta được cung cấp một lưới các ký tự hình chữ nhật. Một “bộ ba” được hình thành bằng cách trước tiên chọn bất kỳ tiểu vùng hình vuông nào của lưới này và sau đó chọn tất cả các ô bên trong hình vuông đó nằm trên hoặc hoàn toàn nằm trên một cạnh của đường chéo của hình vuông."
date: "2026-06-29T09:49:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104668
codeforces_index: "J"
codeforces_contest_name: "2018-2019 ACM-ICPC Central Europe Regional Contest (CERC 18)"
rating: 0
weight: 104668
solve_time_s: 33
verified: true
draft: false
---

[CF 104668J - Matrice](https://codeforces.com/problemset/problem/104668/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 33s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lưới các ký tự hình chữ nhật. Một “bộ ba” được hình thành bằng cách trước tiên chọn bất kỳ tiểu vùng hình vuông nào của lưới này và sau đó chọn tất cả các ô bên trong hình vuông đó nằm trên hoặc hoàn toàn nằm trên một cạnh của đường chéo của hình vuông. Đường chéo có thể là đường chéo chính hoặc đường chéo. Sau quá trình lọc này, chúng tôi giữ lại vùng hình tam giác kết quả. Nếu tất cả các ô trong vùng đó chứa cùng một ký tự và vùng đó chứa ít nhất ba ô thì nó được tính là một bộ ba hợp lệ. 

Nhiệm vụ là đếm xem có bao nhiêu vùng tam giác hợp lệ như vậy tồn tại trên tất cả các lưới con hình vuông có thể có, trên cả hai hướng chéo. 

Kích thước lưới có thể lớn tới 1000 x 1000, nghĩa là lên tới một triệu ô. Một cách tiếp cận ngây thơ thử tất cả các tiểu vùng hình vuông sẽ liên quan đến thứ tự$O(n^3)$hoặc tệ hơn là sự kết hợp các hình vuông và việc kiểm tra từng hình một cách rõ ràng sẽ quá chậm, có khả năng vượt quá$10^{12}$hoạt động. Điều này ngay lập tức gợi ý rằng chúng ta không thể liệt kê trực tiếp tất cả các ô vuông mà thay vào đó phải tính các khoản đóng góp theo cách tổng hợp hơn. 

Một điểm tinh tế trong định nghĩa là các hình vuông khác nhau có thể tạo ra các hình tam giác hình học giống hệt nhau trong lưới, nhưng chúng vẫn được coi là khác biệt nếu chúng đến từ các lựa chọn hình vuông khác nhau. Vì vậy, chúng tôi đang tính các cấu hình chứ không phải các hình dạng độc đáo. 

Các trường hợp cạnh phát sinh khi hình vuông là nhỏ nhất. Hình vuông 1 x 1 hoặc 2 x 2 không tạo ra bộ ba hợp lệ vì vùng hình tam giác thu được sẽ chứa ít hơn ba ô. Một trường hợp cạnh khác là khi một hình vuông hoàn toàn đồng nhất, vì nó tối đa hóa các hình tam giác hợp lệ tiềm năng và bất kỳ lỗi đếm nào cũng có xu hướng đếm quá mức ở đó. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp sẽ là lặp lại tất cả$O(N^2 M^2)$các ô vuông có thể có, và đối với mỗi ô vuông, hãy kiểm tra cả hai hướng chéo rồi xác minh xem tất cả các ô trong vùng tam giác thu được có bằng nhau hay không. Ngay cả khi chúng tôi tính toán trước tổng tiền tố để kiểm tra tính bằng nhau của ký tự, chúng tôi vẫn phải đối mặt với việc lặp lại trên quá nhiều ô vuông và bản thân tổng số ma trận con vuông đã quá lớn trong lưới 1000 x 1000. 

Quan sát quan trọng là mỗi bộ ba hợp lệ được xác định hoàn toàn bởi cấu trúc đỉnh của nó dọc theo đường chéo. Thay vì nghĩ về các hình vuông đầy đủ, chúng tôi diễn giải lại cấu trúc: cố định một ô là một phần của tam giác, chúng tôi có thể mở rộng theo hai hướng vuông góc cho đến khi gặp phải điểm không khớp. Đối với ô bắt đầu cố định và hướng đã chọn (tương ứng với một trong hai đường chéo), kích thước tam giác tối đa được xác định bằng khoảng cách chúng ta có thể mở rộng trong khi vẫn duy trì các ràng buộc ký tự thống nhất. 

Điều này biến bài toán thành phép đếm, đối với mỗi ô, có bao nhiêu kích thước hình vuông hợp lệ theo mỗi hướng chéo. Thay vì liệt kê các ô vuông, chúng tôi tính toán độ dài mở rộng tối đa bằng cách sử dụng phương pháp truyền lan giống như lập trình động dọc theo các đường chéo và sau đó tính tổng các phần đóng góp từ tất cả các ô. 

Sự đơn giản hóa cấu trúc chính là tính hợp lệ chỉ phụ thuộc vào tính nhất quán cục bộ dọc theo các đường chéo và phản đường chéo, điều này cho phép chúng ta tính toán trước các “cánh tay” đồng nhất tối đa theo cả hai hướng. Khi đã biết các độ dài nhánh này, mỗi ô sẽ đóng góp một số hình tam giác hợp lệ bằng hàm của các độ dài này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu trên tất cả các ô vuông |$O(n^4)$|$O(1)$| Quá chậm | 
| Đếm DP theo đường chéo |$O(nm)$|$O(nm)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tách vấn đề thành hai hướng xử lý: hình tam giác dựa trên đường chéo chính và hình tam giác dựa trên đường chéo. Cả hai trường hợp đều đối xứng, vì vậy chúng ta mô tả một trường hợp và áp dụng logic tương tự cho trường hợp kia. 

1. Với mỗi ô, hãy tính xem chúng ta có thể mở rộng một hình vuông có các ký tự giống hệt nhau dọc theo hai hướng vuông góc xác định một hình vuông có hướng chéo bao xa. Điều này được thực hiện bằng cách sử dụng lập trình động trên lưới. Ý tưởng chính là tính toán trước, đối với mỗi ô, đoạn dài nhất của các ký tự giống hệt nhau kết thúc tại hoặc bắt đầu từ ô đó dọc theo hàng và cột, sau đó kết hợp các ràng buộc này để đảm bảo tính nhất quán vuông vắn. 
2. Xây dựng một bảng DP lưu trữ, đối với mỗi ô, độ dài cạnh tối đa có thể có của một hình vuông có góc trên cùng bên trái nằm ở ô đó và các ô của nó đều giống hệt nhau. Đây là bài toán bình phương cực đại cổ điển, nhưng được mở rộng về mặt khái niệm để phản ánh các ràng buộc đối xứng đường chéo. 
3. Đối với mỗi ô và mỗi hướng, số lượng bộ ba hợp lệ do ô đó đóng góp được xác định bằng số lượng kích thước hình vuông có thể được hình thành bắt đầu từ ô đó. Nếu kích thước hình vuông tối đa là$k$, thì nó đóng góp tất cả các kích thước từ 2 đến$k$và mỗi hình vuông như vậy mang lại chính xác một bộ ba hợp lệ cho hướng đường chéo đã chọn. 
4. Tính tổng các đóng góp này trên tất cả các ô và cả hai hướng chéo. 

Tính toán thiết yếu giảm xuống DP bình phương tối đa tiêu chuẩn kết hợp với tiền xử lý theo hướng để đảm bảo tất cả các ô trong các ô vuông ứng viên đều giống hệt nhau. 

### Tại sao nó hoạt động 

Thuật toán hoạt động vì mỗi bộ ba hợp lệ tương ứng duy nhất với một lựa chọn hướng vuông và chéo, và trong mỗi ô vuông, điều kiện “tất cả các ô bằng nhau” tương đương với việc yêu cầu mọi khai triển bình phương con đơn vị vẫn nhất quán. DP đảm bảo rằng nếu một hình vuông có kích thước$k$hợp lệ tại một điểm neo nhất định thì tất cả các ô vuông nhỏ hơn được neo ở đó cũng hợp lệ và không có ô vuông không hợp lệ nào được tính vì bất kỳ sự không khớp nào sẽ ngay lập tức phá vỡ phần mở rộng DP. Điều này tạo ra ánh xạ một-một rõ ràng giữa các trạng thái DP được tính và bộ ba hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    g = [input().strip() for _ in range(n)]

    def count_orientation():
        dp = [[1] * m for _ in range(n)]
        res = 0

        for i in range(n):
            for j in range(m):
                if i > 0 and j > 0 and g[i][j] == g[i-1][j] == g[i][j-1] == g[i-1][j-1]:
                    dp[i][j] = min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1]) + 1
                else:
                    dp[i][j] = 1

                res += max(0, dp[i][j] - 1)

        return res

    def count_antiorientation():
        dp = [[1] * m for _ in range(n)]
        res = 0

        for i in range(n):
            for j in range(m - 1, -1, -1):
                if i > 0 and j + 1 < m and g[i][j] == g[i-1][j] == g[i][j+1] == g[i-1][j+1]:
                    dp[i][j] = min(dp[i-1][j], dp[i][j+1], dp[i-1][j+1]) + 1
                else:
                    dp[i][j] = 1

                res += max(0, dp[i][j] - 1)

        return res

    print(count_orientation() + count_antiorientation())

if __name__ == "__main__":
    solve()
```Việc thực hiện sử dụng hai lượt. Mỗi lượt tính toán một DP bình phương tối đa tiêu chuẩn, một lần cho hướng đường chéo thông thường và một lần cho hướng phản chiếu. Phép lặp kiểm tra xem khối 2 x 2 có đồng nhất hay không; nếu vậy, hình vuông có thể được mở rộng thêm một lớp. 

Sự tinh tế quan trọng là sự tổng kết`dp[i][j] - 1`. Giá trị 1 tương ứng với một hình vuông suy biến không đóng góp bất kỳ bộ ba hợp lệ nào, trong khi các hình vuông lớn hơn đóng góp tất cả các kích thước bộ ba hợp lệ nhỏ hơn được neo tại ô đó. 

Phiên bản chống đường chéo được triển khai bằng cách đảo ngược quá trình truyền tải cột để DP vẫn tham chiếu các trạng thái đã được tính toán. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Lưới:```
2 2
AA
Ad
```Chúng tôi tính toán các giá trị DP cho hướng đường chéo chính. 

| Tế bào | Tính cách
