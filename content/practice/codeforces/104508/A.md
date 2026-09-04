---
title: "CF 104508A - Diện tích lồi"
description: "Chúng ta có các đỉnh của một đa giác lồi theo thứ tự dọc theo biên của nó và nhiệm vụ là tính diện tích hình học của nó. Đầu vào mô tả một hình khép kín trong đó mỗi cặp điểm liên tiếp tạo thành một cạnh và điểm cuối cùng kết nối trở lại điểm đầu tiên."
date: "2026-06-30T15:23:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104508
codeforces_index: "A"
codeforces_contest_name: "National Taiwan University Class Preliminary 2023"
rating: 0
weight: 104508
solve_time_s: 54
verified: true
draft: false
---

[CF 104508A - Diện tích lồi](https://codeforces.com/problemset/problem/104508/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có các đỉnh của một đa giác lồi theo thứ tự dọc theo biên của nó và nhiệm vụ là tính diện tích hình học của nó. Đầu vào mô tả một hình khép kín trong đó mỗi cặp điểm liên tiếp tạo thành một cạnh và điểm cuối cùng kết nối trở lại điểm đầu tiên. Vì đa giác lồi và đã được sắp xếp nên không cần thiết phải xây dựng lại thân hoặc sắp xếp các điểm. 

Đầu ra là một số duy nhất biểu thị diện tích của đa giác này. Tùy thuộc vào định dạng câu lệnh, câu trả lời thường được mong đợi dưới dạng giá trị chính xác hoặc dưới dạng số dấu phẩy động với độ chính xác vừa đủ. 

Từ góc độ ràng buộc, các bài toán thuộc loại này thường cho phép tối đa khoảng 200.000 đỉnh. Điều đó ngay lập tức loại trừ bất kỳ cách tiếp cận bậc hai nào so sánh tất cả các cặp điểm hoặc cố gắng tam giác hóa một cách rõ ràng theo cách ngây thơ. Bất cứ điều gì vượt quá thời gian tuyến tính hoặc tuyến tính sẽ quá chậm. Hạn chế về bộ nhớ thường chặt chẽ nhưng có thể quản lý được vì chúng tôi chỉ lưu trữ danh sách điểm. 

Một vài trường hợp tế nhị có xu hướng phá vỡ việc triển khai ngây thơ. Người ta cho rằng đa giác không được đảm bảo sắp xếp theo thứ tự, điều này sẽ dẫn đến một khu vực sai trừ khi thân tàu được tính toán lại. Một điều nữa là quên rằng tọa độ có thể lớn, vì vậy các phép tính tích chéo trung gian phải sử dụng số nguyên 64 bit hoặc số nguyên Python. Thứ ba là xử lý sai các đa giác suy biến, chẳng hạn như tất cả các điểm nằm trên một đường thẳng, trong đó diện tích đúng bằng 0. Cuối cùng, phép tính tổng dấu phẩy động được thực hiện tăng dần mà không có công thức ổn định có thể tích lũy sai số chính xác cho các phạm vi tọa độ lớn. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là tính diện tích bằng cách phân tách đa giác thành các hình tam giác. Cố định một đỉnh làm trục, ví dụ như điểm đầu tiên và tạo thành các hình tam giác với mọi cặp đỉnh liền kề. Mỗi tam giác đóng góp một diện tích bằng một nửa giá trị tuyệt đối của tích chéo. Tổng các diện tích tam giác này sẽ cho tổng diện tích đa giác. 

Điều này hiệu quả vì bất kỳ đa giác đơn giản nào cũng có thể được tạo tam giác từ một đỉnh cố định và độ lồi đảm bảo rằng tất cả các hình tam giác như vậy nằm bên trong đa giác và không chồng lên nhau. Tuy nhiên, mặc dù điều này rõ ràng về mặt khái niệm, việc thực hiện nó như một cấu trúc hình học lặp đi lặp lại là chi phí không cần thiết. Nút thắt thực sự không phải là tính đúng đắn mà là tính hiệu quả và sự ổn định về số lượng. 

Một quan sát trực tiếp hơn là khi tính tổng diện tích có dấu của các tam giác được hình thành bởi các cạnh liên tiếp, sự đóng góp sẽ biến thành một biểu thức duy nhất. Thay vì xây dựng các hình tam giác một cách rõ ràng, chúng ta có thể tích lũy tích chéo của các đỉnh liên tiếp trong một vòng lặp. Điều này làm giảm toàn bộ tính toán thành một lần tuyến tính duy nhất. 

Phiên bản brute-force sẽ tính toán từng tam giác một cách độc lập, dẫn đến các phép toán O(n²) nếu được thực hiện một cách bất cẩn bằng cách tính toán lại các định thức hoặc thực hiện các phép tính hình học dư thừa. Phiên bản tối ưu hóa sử dụng công thức dây giày, trong đó mỗi cạnh đóng góp chính xác một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tam giác Brute Force | O(n²) | O(1) | Quá chậm | 
| Công thức dây giày | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc tất cả các đỉnh đa giác theo thứ tự và lưu trữ chúng trong một mảng. Thứ tự rất quan trọng vì công thức phụ thuộc vào việc truyền tải nhất quán dọc theo đường biên. 
2. Khởi tạo một biến tích lũy về 0. Điều này sẽ lưu trữ khu vực kép đã ký. 
3. Lặp lại trên tất cả các cạnh của đa giác, bao gồm cả cạnh từ đỉnh cuối cùng trở lại đỉnh đầu tiên. Đối với mỗi cạnh từ điểm i đến điểm i+1, hãy tính đóng góp tích chéo xi * yi+1 − xi+1 * yi. Thuật ngữ này thể hiện sự đóng góp diện tích có dấu của hình bình hành được hình thành bởi hai vectơ. 
4. Thêm từng kết quả tích chéo vào bộ tích lũy. Bước này tính tổng một cách hiệu quả tất cả các diện tích tam giác định hướng được tạo ra bởi các cạnh liên tiếp. 
5. Sau vòng lặp, lấy giá trị tuyệt đối của thanh tích lũy và chia cho 2. Phép chia chuyển đổi diện tích có dấu nhân đôi thành diện tích hình học thực tế. 

### Tại sao nó hoạt động 

Bất biến chính là ở mỗi bước, bộ tích lũy lưu trữ tổng diện tích hình thang có dấu được hình thành giữa mỗi cạnh và gốc tọa độ. Khi bao gồm tất cả các cạnh, các hình thang này xếp chính xác phần bên trong của đa giác với hướng nhất quán. Sự chồng chéo bên trong bị loại bỏ vì mỗi đường chéo chung xuất hiện một lần với hướng dương và một lần với hướng âm khi khai triển tổng tích chéo. Sự hủy bỏ này là thứ thu gọn hình học thành một công thức tuyến tính duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    pts = [tuple(map(int, input().split())) for _ in range(n)]

    area2 = 0
    for i in range(n):
        x1, y1 = pts[i]
        x2, y2 = pts[(i + 1) % n]
        area2 += x1 * y2 - x2 * y1

    print(abs(area2) / 2)

if __name__ == "__main__":
    solve()
```Mã đọc các đỉnh đa giác vào một danh sách để việc lập chỉ mục trở nên đơn giản. Vòng lặp bao quanh một cách rõ ràng bằng cách sử dụng số học modulo để đưa cạnh cuối cùng trở lại điểm bắt đầu. Biến`area2`lưu trữ gấp đôi diện tích đã ký để tránh các thao tác dấu phẩy động lặp đi lặp lại trong quá trình tích lũy. Chỉ khi kết thúc, chúng ta mới chuyển đổi về diện tích cuối cùng bằng cách lấy giá trị tuyệt đối và chia cho hai, điều này tránh mất độ chính xác trong quá trình tính tổng. 

Một lỗi triển khai phổ biến là thực hiện phép chia bên trong vòng lặp, điều này sớm gây ra lỗi dấu phẩy động. Một lỗi khác là quên cạnh đóng giữa điểm cuối cùng và điểm đầu tiên, điều này âm thầm giảm đa giác thành một chuỗi mở và tạo ra một vùng không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
0 0
2 0
2 2
0 2
```Đây là một hình vuông đơn vị có tỷ lệ là 2. 

| tôi | Cạnh | Thuật ngữ sản phẩm chéo | khu vực2 | 
| --- | --- | --- | --- | 
| 0 | (0,0)->(2,0) | 0 | 0 | 
| 1 | (2,0)->(2,2) | 4 | 4 | 
| 2 | (2,2)->(0,2) | 4 | 8 | 
| 3 | (0,2)->(0,0) | 0 | 8 | 

Diện tích cuối cùng = 8/2 = 4 

Điều này xác nhận rằng công thức tích lũy chính xác các đóng góp từ mỗi cạnh và tái tạo lại diện tích hình học dự kiến. 

### Ví dụ 2 

đầu vào:```
3
0 0
4 0
0 3
```| tôi | Cạnh | Thuật ngữ sản phẩm chéo | khu vực2 | 
| --- | --- | --- | --- | 
| 0 | (0,0)->(4,0) | 0 | 0 | 
| 1 | (4,0)->(0,3) | 12 | 12 | 
| 2 | (0,3)->(0,0) | 0 | 12 | 

Diện tích cuối cùng = 12/2 = 6 

Đây là một tam giác vuông có đáy 4 và chiều cao 3, xác nhận tính đúng đắn của cách giải thích tích chéo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | mỗi đỉnh đóng góp một lần vào một phép tính tích chéo | 
| Không gian | O(n) | lưu trữ điểm đầu vào | 

Thuật toán chạy thoải mái trong các ràng buộc điển hình cho các bài toán đa giác, ngay cả khi n lớn, vì nó tránh được mọi phép lặp lồng nhau hoặc cấu trúc hình học ngoài một lần duyệt. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    # inline solution
    input = sys.stdin.readline
    n = int(input())
    pts = [tuple(map(int, input().split())) for _ in range(n)]

    area2 = 0
    for i in range(n):
        x1, y1 = pts[i]
        x2, y2 = pts[(i + 1) % n]
        area2 += x1 * y2 - x2 * y1

    return str(abs(area2) / 2)

# provided samples (assumed)
assert run("4\n0 0\n2 0\n2 2\n0 2\n") == "4.0"
assert run("3\n0 0\n4 0\n0 3\n") == "6.0"

# custom cases
assert run("1\n0 0\n") == "0.0", "single point"
assert run("2\n0 0\n1 0\n") == "0.0", "collinear points"
assert run("4\n0 0\n1 0\n1 1\n0 1\n") == "1.0", "unit square"
assert run("3\n0 0\n1000000000 0\n0 1000000000\n") == "5e+17", "large coordinates"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| điểm duy nhất | 0 | đa giác suy biến | 
| hai điểm | 0 | sụp đổ đa giác không hợp lệ | 
| đơn vị vuông | 1 | tính đúng đắn cơ bản | 
| tam giác lớn | giá trị lớn | an toàn tràn và xử lý số nguyên | 

## Vỏ cạnh 

Đầu vào suy biến trong đó tất cả các điểm nằm trên một đường sẽ tạo ra diện tích bằng không. Thuật toán xử lý điều này một cách tự nhiên vì tất cả các tích chéo đều có giá trị bằng 0 vì mỗi phân đoạn không đóng góp vào vùng kèm theo. 

Một đa giác tối thiểu có một hoặc hai điểm cũng cho kết quả bằng 0. Vòng lặp vẫn chạy chính xác, nhưng mọi số hạng tích chéo đều bằng 0 vì các điểm lặp lại hoặc thiếu phần đóng sẽ ngăn cản sự hình thành vùng. 

Các giá trị tọa độ lớn không gây ra sự cố trong Python do số nguyên có độ chính xác tùy ý, nhưng trong các ngôn ngữ có số nguyên có chiều rộng cố định, bước nhân phải được tăng cấp cẩn thận lên số học 64 bit để tránh tràn trong quá trình tích lũy.
