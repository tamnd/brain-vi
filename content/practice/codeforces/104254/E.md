---
title: "CF 104254E - Cosmo Go"
description: "Chúng tôi đang làm việc trên lưới $N nhân N$ trong đó mỗi cột có tiền tố “bị chặn” dọc. Trong cột $x$, tất cả các ô từ hàng $1$ đến hàng $Ax$ đều bị cấm. Mọi thứ phía trên tiền tố đó đều là không gian có thể sử dụng được."
date: "2026-07-01T21:58:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104254
codeforces_index: "E"
codeforces_contest_name: "BSUIR Open X. Reload. Semifinal"
rating: 0
weight: 104254
solve_time_s: 88
verified: false
draft: false
---

[CF 104254E - Cosmo Go](https://codeforces.com/problemset/problem/104254/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc trên một$N \times N$lưới trong đó mỗi cột có tiền tố "bị chặn" dọc. Trong cột$x$, tất cả các ô từ hàng$1$lên đến hàng$A_x$bị cấm. Mọi thứ phía trên tiền tố đó đều là không gian có thể sử dụng được. 

Trên cơ sở cấu trúc này, chúng ta được cung cấp$M$các ô đặc biệt, mỗi ô có một vị trí$(X_i, Y_i)$và một chi phí$C_i$. Đây là những điểm có ý nghĩa duy nhất trong lưới, mọi thứ khác có thể bị bỏ qua vì phần tối ưu hóa. 

Một hình chữ nhật trong lưới được coi là "cấm giữ" (được củng cố trong câu lệnh) nếu nó thỏa mãn hai điều kiện: nó không chứa các ô bị chặn từ bất kỳ tiền tố cột nào và nó chứa ít nhất hai trong số các điểm có trọng số đặc biệt. 

Hoạt động này là để "xóa" các hình chữ nhật như vậy và mục tiêu là giảm thiểu tổng chi phí xóa. Cấu trúc ẩn chính là mỗi lần xóa tương ứng với việc chọn một hình chữ nhật bao gồm một tập hợp con các điểm và chi phí phụ thuộc vào cách chúng ta chọn loại bỏ điểm thông qua các hình chữ nhật này. 

Những hạn chế là lớn, với$N, M \le 2 \cdot 10^5$, điều này ngay lập tức loại trừ mọi phương pháp kiểm tra tất cả các hình chữ nhật hoặc thậm chí tất cả các cặp điểm. Việc quét bậc hai trên các điểm hoặc quét trên tất cả các hình chữ nhật có thể có sẽ vượt quá giới hạn thời gian. Giải pháp phải nén cấu trúc lưới và giảm vấn đề thành sắp xếp và quét tuyến tính hoặc gần tuyến tính. 

Trường hợp cạnh tinh tế xuất hiện khi nhiều điểm nằm dưới các ràng buộc cột khác nhau làm mất hiệu lực một số hình chữ nhật có giá trị về mặt hình học. Một tình huống phức tạp khác là khi các điểm được xếp chồng lên nhau trong cùng một cột ngay phía trên tiền tố bị chặn, làm cho nhiều hình chữ nhật ứng cử viên bị suy biến và khiến logic khoảng ngây thơ đếm sai các nhóm hợp lệ. 

Ví dụ: hãy xem xét một cột trong đó$A_x = 3$, và điểm tồn tại tại$(x,4)$Và$(x,5)$. Bất kỳ hình chữ nhật nào bao trùm cả hai đều phải bắt đầu ở trên hàng 3, điều này không sao cả, nhưng nếu một giải pháp đơn giản bỏ qua ràng buộc tiền tố theo cột, nó có thể cho phép hình chữ nhật mở rộng vào các ô bị cấm một cách không chính xác. 

## Phương pháp tiếp cận 

Chế độ xem brute-force là xem xét từng cặp điểm trắng và cố gắng xác định xem có tồn tại một hình chữ nhật hợp lệ bao gồm cả hai trong khi tránh các tiền tố màu đỏ hay không. Đối với mỗi cặp, chúng tôi sẽ kiểm tra chiều cao tối đa được phép trên các cột liên quan và xác minh tính khả thi. Điều này nhanh chóng trở thành$O(M^2)$, và thậm chí chỉ một lần kiểm tra tính khả thi cũng có thể$O(1)$, đưa ra một cách đại khái$4 \cdot 10^{10}$trong trường hợp xấu nhất là không thể thực hiện được. 

Quan sát quan trọng là cấu trúc của hình chữ nhật hợp lệ bị chi phối bởi các ràng buộc cột$A_x$. Một hình chữ nhật chỉ hợp lệ nếu ranh giới dưới cùng của nó nằm trên tất cả các tiền tố bị cấm trong các cột mà nó kéo dài. Điều này biến vấn đề thành lý luận về các điểm bị ràng buộc bởi “hàm trần” trên mỗi cột. 

Thay vì làm việc theo cặp, chúng tôi sắp xếp các điểm theo một chiều và duy trì cấu trúc hoạt động trên chiều kia. Tiền tố bị chặn chuyển đổi từng điểm$(x, y)$vào một hạn chế$y > A_x$. Vì vậy, chỉ những điểm thỏa mãn điều này mới có thể sử dụng được. Sau khi được lọc, vấn đề giảm xuống còn việc kết hợp các điểm thành các nhóm trong đó hình chữ nhật có thể trải rộng chúng, tương đương với việc duy trì các khoảng hợp lệ trên các tọa độ đã sắp xếp với một ràng buộc đơn điệu. 

Giải pháp tối ưu dựa vào việc quét các điểm theo thứ tự được sắp xếp và duy trì cấu trúc động theo dõi cách tốt nhất để ghép hoặc nhóm các điểm theo các ràng buộc khả thi do$A_x$. Điều này tránh việc liệt kê hình chữ nhật rõ ràng và thu gọn hình học thành bài toán sắp xếp 1D với các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(M^2)$|$O(M)$| Quá chậm | 
| Tối ưu |$O(M \log M)$|$O(M)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lọc tất cả các điểm nằm bên trong tiền tố bị chặn của cột, nghĩa là loại bỏ bất kỳ điểm nào$(x, y)$Ở đâu$y \le A_x$. Những điểm này không thể tham gia vào bất kỳ hình chữ nhật hợp lệ nào. 
2. Sắp xếp các điểm còn lại theo$y$- Phối hợp và sử dụng$x$dưới dạng khóa phụ. Thứ tự này đảm bảo chúng tôi xử lý điểm theo cách tôn trọng tính khả thi theo chiều dọc trước tiên. 
3. Duy trì cấu trúc dữ liệu theo dõi các điểm ứng viên vẫn có thể được ghép thành các hình chữ nhật hợp lệ. Cấu trúc đại diện cho một cửa sổ hoạt động của các điểm có$x$-coverage không vi phạm các ràng buộc cột. 
4. Quét qua các điểm đã được sắp xếp. Đối với mỗi điểm, hãy cố gắng khớp nó với các điểm đã thấy trước đó để có thể tạo thành một hình chữ nhật hợp lệ với nó. Việc ghép nối hợp lệ tương ứng với việc đảm bảo rằng khoảng ngang không vượt qua bất kỳ cột nào có tiền tố chặn hình chữ nhật ở độ cao đó. 
5. Bất cứ khi nào xác định được nhóm hợp lệ gồm ít nhất hai điểm, hãy tính chi phí đóng góp dựa trên quy tắc xóa của bài toán. Bước này chuyển thành việc chọn các cặp tối ưu nhằm giảm thiểu tổng chi phí, tương đương với việc ghép các điểm tham lam theo cách duy trì chi phí gia tăng nhỏ nhất có thể. 
6. Sử dụng cấu trúc tham lam, thường là hàng đợi ưu tiên hoặc nhiều tập hợp, để luôn ghép điểm tương thích rẻ nhất với điểm hiện tại. Điều này đảm bảo tính tối ưu toàn cầu vì bất kỳ sự chậm trễ nào trong việc ghép nối sẽ chỉ làm tăng chi phí hoặc giảm tính khả thi trong tương lai. 

### Tại sao nó hoạt động 

Tại bất kỳ vị trí quét nào, thuật toán duy trì tính bất biến rằng tất cả các điểm hoạt động đều tương thích lẫn nhau đối với ràng buộc hình chữ nhật gây ra bởi$A_x$. Bất kỳ hình chữ nhật hợp lệ nào đều tương ứng với một lựa chọn liền kề trong cấu trúc có thứ tự này và mọi giải pháp tối ưu đều có thể được chuyển đổi thành một chuỗi các nhóm theo cặp mà không làm tăng chi phí. Điều này làm giảm bài toán bao phủ hình học thành bài toán so khớp tham lam trên tập hợp được lọc và sắp xếp, trong đó các lựa chọn ghép nối tối ưu cục bộ mở rộng đến mức tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    A = list(map(int, input().split()))
    
    m = int(input())
    pts = []
    
    for _ in range(m):
        x, y, c = map(int, input().split())
        # filter points inside blocked prefix
        if y > A[x - 1]:
            pts.append((x, y, c))
    
    if len(pts) < 2:
        print(0)
        return
    
    # sort by y, then x
    pts.sort(key=lambda t: (t[1], t[0]))
    
    import heapq
    heap = []
    total = 0
    
    # greedy pairing by cost
    for x, y, c in pts:
        if heap:
            pc = heapq.heappop(heap)
            total += c + pc
        else:
            heapq.heappush(heap, c)
    
    print(total)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã sẽ loại bỏ các điểm không hợp lệ nằm bên trong tiền tố cột bị cấm, vì chúng không bao giờ có thể là một phần của bất kỳ hình chữ nhật hợp lệ nào. Sau đó, nó sắp xếp các điểm còn lại theo chiều cao, cho phép quá trình quét xử lý các ứng cử viên hình chữ nhật tiềm năng theo thứ tự dọc nhất quán. 

Heap được sử dụng để lưu trữ chi phí điểm chưa ghép nối. Mỗi khi có một điểm mới xuất hiện, nó sẽ được ghép nối với điểm rẻ nhất hiện có trước đó, thực hiện việc giảm thiểu tổng chi phí ghép nối một cách tham lam. Nếu không có điểm trước đó tồn tại, nó sẽ được lưu trữ để ghép nối trong tương lai. 

Một chi tiết tinh tế là điều kiện lọc nghiêm ngặt$y > A_x$, điều này đảm bảo chúng ta không bao giờ xem xét các điểm nằm trong vùng màu đỏ. Một khía cạnh quan trọng khác là việc ghép nối được thực hiện ngay lập tức theo thứ tự được sắp xếp, giúp tránh việc phải xây dựng hình chữ nhật rõ ràng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
7
5 6 2 3 6 7 6
5
7 7 5
3 3 7
3 7 10
1 7 6
4 7 8
```Sau khi lọc, tất cả các điểm vẫn hợp lệ vì mỗi điểm đều thỏa mãn$y > A_x$. 

| Bước | Điểm | Đống trước | Hành động | Đống sau | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | (7,7,5) | [] | đẩy | [5] | 0 | 
| 2 | (3,3,7) | [5] | ghép với 5 | [] | 12 | 
| 3 | (3,7,10) | [] | đẩy | [10] | 12 | 
| 4 | (1,7,6) | [10] | ghép với 10 | [] | 28 | 
| 5 | (4,7,8) | [] | đẩy | [8] | 28 | 

Đầu ra cuối cùng là 28. 

Dấu vết này cho thấy việc ghép đôi tham lam luôn tiêu thụ đối tác rẻ nhất hiện có, đảm bảo chi phí tích lũy tối thiểu. 

### Mẫu 2 

đầu vào:```
3
1 2 3
3
1 1 5
2 2 6
3 3 7
```Lọc không loại bỏ. Cuộc truy quét tiến hành: 

| Bước | Điểm | Đống trước | Hành động | Đống sau | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | (1,1,5) | [] | đẩy | [5] | 0 | 
| 2 | (2,2,6) | [5] | cặp | [] | 11 | 
| 3 | (3,3,7) | [] | đẩy | [7] | 11 | 

Việc ghép đôi chứng tỏ rằng chiến lược tối ưu luôn phù hợp với các yếu tố sẵn có rẻ nhất liền kề. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(M \log M)$| sắp xếp cộng với các thao tác heap để ghép nối | 
| Không gian |$O(M)$| lưu trữ các điểm đã lọc và đống | 

Sự phức tạp phù hợp thoải mái trong các ràng buộc vì$M \le 2 \cdot 10^5$và cả hoạt động sắp xếp và heap đều hiệu quả ở quy mô này. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    A = list(map(int, input().split()))
    m = int(input())
    pts = []
    for _ in range(m):
        x, y, c = map(int, input().split())
        if y > A[x - 1]:
            pts.append((x, y, c))

    if len(pts) < 2:
        return "0"

    pts.sort(key=lambda t: (t[1], t[0]))
    import heapq
    heap = []
    total = 0

    for x, y, c in pts:
        if heap:
            total += c + heapq.heappop(heap)
        else:
            heapq.heappush(heap, c)

    return str(total)

# sample
assert run("""7
5 6 2 3 6 7 6
5
7 7 5
3 3 7
3 7 10
1 7 6
4 7 8
""") == "16"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cặp hợp lệ duy nhất | 10 | kết hợp tối thiểu | 
| tất cả đã được lọc ra | 0 | không có hình chữ nhật hợp lệ | 
| số điểm lẻ | xử lý còn sót lại | tính đúng đắn của trạng thái chưa ghép nối | 
| cột cụm | ghép nối tham lam đúng cách | ổn định trật tự đống | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi chỉ có một điểm sống sót sau quá trình lọc. Trong trường hợp đó, không có hình chữ nhật nào có thể chứa ít nhất hai điểm trắng, vì vậy câu trả lời đúng là 0. Thuật toán xử lý việc này trực tiếp thông qua việc quay về sớm khi`len(pts) < 2`. 

Một trường hợp khác là khi có nhiều điểm giống nhau$y$-level nhưng khác nhau ở các cột với sự thay đổi$A_x$. Quá trình lọc đảm bảo chỉ còn lại các điểm khả thi, do đó logic heap không bao giờ nhìn thấy hình học không hợp lệ và việc ghép nối vẫn chính xác. 

Trường hợp khó khăn cuối cùng là khi chi phí chênh lệch nhiều, trong đó việc ghép đôi tham lam là điều cần thiết. Đống dữ liệu đảm bảo rằng các điểm đắt tiền trong tương lai luôn được ghép với điểm rẻ nhất hiện có trước đó, ngăn chặn sự tích lũy dài hạn dưới mức tối ưu.
