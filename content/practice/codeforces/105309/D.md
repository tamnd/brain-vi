---
title: "CF 105309D - Ngũ cốc Grids III (Phiên bản dễ dàng)"
description: "Chúng ta được yêu cầu xây dựng một lưới nhị phân $n nhân n$ bằng cách sử dụng chính xác $k$ số một và $n^2 - k$ số không. Yêu cầu duy nhất trên lưới là cấu trúc: khi chúng ta xem tất cả các hàng dưới dạng chuỗi nhị phân và tất cả các cột dưới dạng chuỗi nhị phân, số lượng hàng riêng biệt cộng với số lượng…"
date: "2026-06-23T14:52:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105309
codeforces_index: "D"
codeforces_contest_name: "CerealCodes III Novice Division"
rating: 0
weight: 105309
solve_time_s: 85
verified: false
draft: false
---

[CF 105309D - Ngũ cốc Grids III (Phiên bản dễ dàng)](https://codeforces.com/problemset/problem/105309/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xây dựng một$n \times n$lưới nhị phân sử dụng chính xác$k$những cái và$n^2 - k$số không. Yêu cầu duy nhất trên lưới là về cấu trúc: khi chúng ta xem tất cả các hàng dưới dạng chuỗi nhị phân và tất cả các cột dưới dạng chuỗi nhị phân, số hàng riêng biệt cộng với số cột riêng biệt phải nhiều nhất là 5. 

Vì vậy, ràng buộc không phải là về các mẫu cục bộ hoặc sự kề cận, mà là về việc nén sự phân tập. Chúng tôi được phép đặt những cái đó ở bất kỳ đâu miễn là lưới có rất ít loại hàng và loại cột riêng biệt được kết hợp. 

Khó khăn chính đó là$n$có thể lớn tới 1000, vì vậy mọi cách xây dựng đều phải tuyến tính hoặc gần tuyến tính trong$n^2$. Tuy nhiên, bản thân đầu ra đã$n^2$, Vì thế$O(n^2)$việc xây dựng là không thể tránh khỏi và được mong đợi. 

Điều tinh tế chính là chúng ta phải kiểm soát đồng thời cả phân tập hàng và phân tập cột. Cách tiếp cận đơn giản là xây dựng các hàng một cách độc lập có thể dễ dàng tạo ra quá nhiều mẫu cột riêng biệt. 

Một vài trường hợp thất bại xuất hiện ngay lập tức nếu chúng ta thử xây dựng một cách bất cẩn. Ví dụ, điền từng hàng một cho đến khi chúng ta cạn kiệt$k$có xu hướng tạo ra nhiều cột riêng biệt vì ranh giới giữa các hàng được lấp đầy và không được lấp đầy tạo ra mô hình “cầu thang”. 

Một ý tưởng có vấn đề khác là xen kẽ các hàng như$1010...$Và$0101...$. Mặc dù tính đa dạng của hàng là nhỏ nhưng tính đa dạng của cột lại tăng lên$n$, vi phạm giới hạn cấp bậc. 

Thách thức là thiết kế một lưới trong đó các hàng và cột đều có tính lặp lại cao và số lượng riêng biệt kết hợp của chúng vẫn bị giới hạn. 

## Phương pháp tiếp cận 

Tư duy vũ phu coi đây là một vấn đề thỏa mãn ràng buộc: cố gắng gán cho mỗi ô một giá trị và duy trì một tập hợp các hàng và cột riêng biệt đang chạy, quay lại bất cứ khi nào số vượt quá 5. Điều này rõ ràng là theo cấp số nhân trong$n^2$bởi vì mỗi lựa chọn ô có khả năng thay đổi chữ ký hàng và cột và không có cấu trúc cắt tỉa nào đảm bảo kết thúc sớm. Ngay cả đối với$n = 20$, điều này đã không thể thực hiện được. 

Quan sát quan trọng là chúng ta không cần tối ưu hóa bất cứ điều gì hoặc tìm kiếm. Chúng ta chỉ cần sự tồn tại của một cấu trúc rất đơn giản. Nếu chúng ta có thể đảm bảo rằng tất cả các hàng là một trong nhiều mẫu cố định và tất cả các cột cũng thuộc một tập hợp cố định nhỏ thì điều kiện sẽ tự động được thỏa mãn. 

Cách đơn giản nhất để buộc tính đa dạng của cả hàng và cột không đổi là làm cho lưới gần như đồng nhất, với tất cả các biến thể tập trung trong một hình chữ nhật duy nhất. Nếu tất cả các hàng giống hệt nhau ngoại trừ có thể có một hoặc hai hàng và tất cả các cột đều giống hệt nhau ngoại trừ có thể có một hoặc hai cột thì cả loại hàng và loại cột vẫn bị giới hạn. 

Một cấu trúc rõ ràng là chia lưới thành hai khối lớn đồng nhất: một khối chứa đầy các số 1, phần còn lại là các số 0. Điều này tạo ra tối đa hai loại hàng riêng biệt và nhiều nhất là hai loại cột riêng biệt, mang lại tổng thứ hạng siêu cao nhất là 4, đáp ứng yêu cầu. 

Sau đó, chúng tôi điều chỉnh kích thước của hình chữ nhật tổng hợp sao cho nó chứa chính xác$k$những cái đó. 

Cấu trúc trở thành: chọn tiền tố của các hàng và cột tạo thành hình chữ nhật trên cùng bên trái có diện tích được điều chỉnh chính xác hoặc một chút cho phù hợp$k$. Bởi vì chúng tôi được phép sắp xếp bất kỳ số 1 nào, chúng tôi có thể chỉ cần điền tiền tố hình chữ nhật một cách tham lam từng hàng bên trong vùng tiền tố và giữ mọi thứ khác bằng 0. Điều này vẫn duy trì một số lượng giới hạn các mẫu hàng và cột riêng biệt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (xây dựng lưới quay lui) | hàm mũ trong$n^2$|$O(n^2)$| Quá chậm | 
| Cấu trúc xây dựng hình chữ nhật |$O(n^2)$|$O(1)$thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng một lưới trong đó tất cả các lưới được đặt trong một vùng liền kề trên cùng bên trái. 

1. Khởi tạo một$n \times n$lưới chứa đầy số không. Điều này đảm bảo một mẫu đường cơ sở duy nhất cho tất cả các hàng và cột trước khi sửa đổi. 
2. Bắt đầu điền từ ô trên cùng bên trái, quét từng hàng, từ trái sang phải. 
3. Đặt những cái đó cho đến khi chính xác$k$những cái đã được viết. Điều này đảm bảo ràng buộc về số lượng được thỏa mãn mà không cần cân bằng phức tạp. 
4. Dừng lại ngay một lần$k$những cái được đặt. Tất cả các ô còn lại ở mức 0. 
5. Xuất lưới. 

Hiệu ứng cấu trúc quan trọng của quá trình này là tập hợp các hàng chỉ có thể thay đổi ở nhiều nhất một điểm chuyển tiếp: các hàng phía trên hàng được điền một phần cuối cùng chứa đầy các hàng cho đến một số tiền tố và tất cả các hàng bên dưới đều là số 0. Tương tự, các cột hoạt động đối xứng: các cột trước cột được điền một phần cuối cùng chứa các cột trong tất cả các hàng được điền và các cột sau cột đó đều là số 0. 

### Tại sao nó hoạt động 

Lưới được tạo ra có nhiều nhất hai loại hàng: các hàng hoàn toàn bằng 0 và có thể một hàng chứa tiền tố là các hàng một. Không có mẫu hàng nào khác có thể xuất hiện vì quá trình điền được tiến hành trong một lần quét tuyến tính. Tương tự, có nhiều nhất hai loại cột: cột hoàn toàn bằng 0 và có thể một cột có tiền tố một phần là cột 1. Do đó tổng số hàng và cột riêng biệt nhiều nhất là 4, thỏa mãn yêu cầu nhiều nhất là 5. Việc xây dựng không phụ thuộc vào$k$ngoại trừ việc quá trình điền diễn ra bao xa, vì vậy nó hoạt động với tất cả các đầu vào hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    
    grid = [["0"] * n for _ in range(n)]
    
    for i in range(n):
        for j in range(n):
            if k == 0:
                break
            grid[i][j] = "1"
            k -= 1
        if k == 0:
            break
    
    for row in grid:
        print("".join(row))

if __name__ == "__main__":
    solve()
```Giải pháp xây dựng lưới dưới dạng danh sách các mảng ký tự để đột biến hiệu quả. Vòng lặp kép thực hiện thao tác điền đơn giản vào phía trên bên trái. Điều kiện dừng sớm đảm bảo chúng ta không đổ đầy một lần$k$những cái được đặt. 

Chi tiết triển khai chính là ngắt chính xác cả hai vòng lặp khi$k$đạt tới số không. Không phá vỡ vòng lặp bên ngoài vẫn có kết quả chính xác nhưng lãng phí thời gian; việc không phá vỡ vòng lặp bên trong đúng cách có thể ghi đè sai số 0 sau khi hoàn thành ở một số biến thể. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4, k = 5
```Chúng tôi điền từng hàng. 

| Bước | Vị trí (i,j) | Hành động | Còn lại k | 
| --- | --- | --- | --- | 
| 1 | (0,0) | nơi 1 | 4 | 
| 2 | (0,1) | nơi 1 | 3 | 
| 3 | (0,2) | nơi 1 | 2 | 
| 4 | (0,3) | nơi 1 | 1 | 
| 5 | (1,0) | nơi 1 | 0 | 

Lưới cuối cùng:```
1111
1000
0000
0000
```Điều này cho thấy chỉ tồn tại một hàng chuyển tiếp và một cột chuyển tiếp, tạo ra tối đa hai loại hàng và hai loại cột. 

### Ví dụ 2 

đầu vào:```
n = 3, k = 4
```| Bước | Vị trí (i,j) | Hành động | Còn lại k | 
| --- | --- | --- | --- | 
| 1 | (0,0) | nơi 1 | 3 | 
| 2 | (0,1) | nơi 1 | 2 | 
| 3 | (0,2) | nơi 1 | 1 | 
| 4 | (1,0) | nơi 1 | 0 | 

Lưới cuối cùng:```
111
100
000
```Điều này một lần nữa tạo ra tối đa hai mẫu hàng và hai mẫu cột, xác nhận giới hạn siêu cấp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| Chúng tôi truy cập các ô theo thứ tự hàng lớn cho đến khi đặt$k$những cái | 
| Không gian |$O(n^2)$| Lưu trữ lưới | 

Những ràng buộc cho phép$n$lên tới 1000, vì vậy$n^2 = 10^6$các ô, điều này có thể dễ dàng thực hiện được trong Python với các phép gán và đầu ra đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = output
    try:
        solve()
    finally:
        sys.stdout = old_stdout
    return output.getvalue().strip()

# provided sample
assert run("4 12\n") in ["1111\n1111\n1111\n1100", "1111\n1111\n1111\n1111"], "sample 1 (format tolerant)"

# minimum case
assert run("1 0\n") == "0", "n=1,k=0"
assert run("1 1\n") == "1", "n=1,k=1"

# small rectangle behavior
assert run("3 4\n") in ["111\n100\n000", "111\n100\n000"], "small k fill"

# full grid
assert run("2 4\n") == "11\n11", "full ones grid"

# empty grid
assert run("3 0\n") == "000\n000\n000", "all zeros"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 | 0 | lưới tối thiểu | 
| 1 1 | 1 | ô đơn đầy | 
| 3 0 | tất cả số không | xây dựng trống | 
| 2 4 | đầy đủ | bão hòa hoàn toàn | 
| 3 4 | điền một phần | dừng sớm đúng đắn | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi$k = 0$. Thuật toán không bao giờ đi vào vòng lặp điền và trực tiếp tạo ra lưới hoàn toàn bằng 0. Điều này bảo toàn chính xác một loại hàng và một loại cột, giữ thứ hạng siêu ở mức tối thiểu. 

Khi$k = n^2$, vòng lặp lấp đầy toàn bộ lưới. Mỗi ô trở thành 1, tạo ra một loại hàng và một loại cột. Đây là cấu hình đồng đều nhất có thể và thỏa mãn được ràng buộc một cách tầm thường. 

Khi$k$dừng chính xác tại một ranh giới hàng, vùng được tô đầy tạo thành một hình chữ nhật hoàn hảo, tạo ra chính xác hai loại hàng: hàng đầy đủ và hàng không đầy đủ. Sự đa dạng của cột cũng bị hạn chế tương tự. 

Khi$k$dừng ở giữa một hàng, chỉ một hàng trở thành hàng tiền tố hỗn hợp. Đây là hàng duy nhất giới thiệu mẫu thứ ba, nhưng vẫn giữ tổng số hàng và cột riêng biệt được giới hạn dưới 5.
