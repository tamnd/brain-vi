---
title: "CF 1635A - Tối thiểu hoặc Tổng"
description: "Chúng ta được cho một số mảng, mỗi mảng chứa các số nguyên nhỏ không âm. Đối với mỗi mảng, chúng ta được phép chọn liên tục hai vị trí khác nhau và thay thế cả hai giá trị bằng số mới, miễn là OR theo bit của cặp đã chọn không thay đổi sau khi thay thế."
date: "2026-06-10T04:40:00+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1635
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 772 (Div. 2)"
rating: 800
weight: 1635
solve_time_s: 69
verified: true
draft: false
---

[CF 1635A - Tối thiểu hoặc Tổng](https://codeforces.com/problemset/problem/1635/A) 

**Đánh giá:** 800 
**Tags:** bitmasks, tham lam 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số mảng, mỗi mảng chứa các số nguyên nhỏ không âm. Đối với mỗi mảng, chúng ta được phép chọn liên tục hai vị trí khác nhau và thay thế cả hai giá trị bằng số mới, miễn là OR theo bit của cặp đã chọn không thay đổi sau khi thay thế. Mục tiêu là làm cho tổng của mảng càng nhỏ càng tốt sau bất kỳ số thao tác nào như vậy. 

Đối tượng chính ở đây là bitwise OR trên toàn bộ mảng. Mọi thao tác đều bảo toàn OR của cặp đã chọn và vì mọi phần tử đều có thể tham gia vào các phép toán liên tục nên OR toàn cục của mảng không bao giờ thay đổi. Điều đó có nghĩa là mảng cuối cùng vẫn phải có OR tổng thể giống như đầu vào ban đầu. 

Các ràng buộc rất nhỏ: mỗi trường hợp thử nghiệm có tối đa 100 phần tử và các giá trị được giới hạn dưới 2^30. Điều này gợi ý rõ ràng rằng mọi giải pháp O(n^2) hoặc O(n * 30) cho mỗi trường hợp thử nghiệm đều đủ nhanh và chúng ta nên tập trung vào lý luận theo bit thay vì mô phỏng các hoạt động. 

Một cạm bẫy tinh vi là giả sử chúng ta có thể tự do phân phối lại các bit một cách độc lập giữa các phần tử. Ví dụ, người ta có thể nghĩ rằng chúng ta luôn có thể tập trung các bit vào một phần tử duy nhất và loại bỏ các phần tử khác. Điều này không phải lúc nào cũng nhất quán với quy tắc bảo toàn OR trừ khi chúng ta tôn trọng cách các bit có thể được chia thành nhiều số trong khi vẫn duy trì các ràng buộc theo cặp trong quá trình hoạt động. 

Trường hợp cạnh thứ hai là mảng có một phần tử trội duy nhất chứa tất cả các bit. Trong những trường hợp như vậy, không thể rút gọn được và kết quả vẫn giữ nguyên là tổng. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ cố gắng mô phỏng tất cả các hoạt động hợp lệ, liên tục chọn các cặp và phân phối lại các bit theo mọi cách có thể để bảo toàn OR. Điều này nhanh chóng trở nên khó giải quyết vì mỗi thao tác đưa ra nhiều trạng thái có thể xảy ra và số lượng chuỗi thao tác tăng theo cấp số nhân. Ngay cả khi chỉ có 100 phần tử, không gian trạng thái vẫn bùng nổ. 

Quan sát quan trọng là bất biến duy nhất thực sự quan trọng là OR theo bit của toàn bộ mảng. Đặt OR đó được ký hiệu là S. Cho dù chúng ta thực hiện thao tác nào, S vẫn cố định. 

Bây giờ hãy xem mảng cuối cùng trông như thế nào. Mỗi phần tử đóng góp một số tập hợp con các bit và sự kết hợp của tất cả các tập hợp con này phải bằng S. Tổng được giảm thiểu khi chúng ta tránh trùng lặp các bit trên nhiều phần tử, bởi vì bất kỳ bit lặp lại nào cũng làm tăng tổng một cách không cần thiết. Cấu hình tối ưu là đảm bảo rằng mỗi tập bit trong S xuất hiện trong chính xác một phần tử và không có phần tử nào mang các bit dư thừa ngoài cấu trúc đó. 

Điều này dẫn đến sự đơn giản hóa mạnh mẽ: chúng ta muốn phân phối các bit của S trên các phần tử trong khi tôn trọng rằng mỗi phần tử phải được dẫn xuất từ ​​các giá trị ban đầu thông qua các phép toán được phép. Cấu trúc tối ưu có thể đạt được hóa ra là chúng ta luôn có thể giảm mảng thành có chính xác một phần tử bằng S và tất cả các phần tử khác bằng 0, ngoại trừ trường hợp các giá trị ban đầu đã hạn chế phân phối lại tối thiểu. Tuy nhiên, lý do cẩn thận hơn cho thấy một thực tế thậm chí còn đơn giản hơn: tổng tối thiểu có thể đạt được là tổng của tất cả các phần tử sau khi thay thế từng phần tử bằng OR của toàn bộ mảng, nhưng được phân phối ở mức tối thiểu, dẫn đến đối số đóng góp bit tham lam. 

Một dẫn xuất chuẩn và rõ ràng hơn như sau: mỗi bit đóng góp độc lập. Đối với mỗi bit, nếu nó xuất hiện trong k phần tử, nó có thể được giảm xuống để chỉ một phần tử giữ bit đó trong khi các phần tử khác thả nó về 0 mà không làm thay đổi tính khả thi OR toàn cục. Do đó, mỗi bit đóng góp chính xác giá trị của nó một lần vào tổng cuối cùng. Vì các bit chồng chéo không làm tăng chi phí nên tổng tối ưu chỉ đơn giản trở thành giá trị của OR cộng với các đóng góp vốn không thể tránh khỏi trong cấu trúc, điều này đơn giản hóa thành:

Câu trả lời là tổng của tất cả các phần tử trừ đi thứ gì đó phụ thuộc vào sự trùng lặp, nhưng được biết một cách trực tiếp và chính xác hơn từ bài toán này: tổng tối thiểu bằng OR của tất cả các phần tử cộng với tổng của tất cả các phần tử đã loại bỏ phần đóng góp dư thừa, giúp đơn giản hóa hơn nữa tổng các phần tử sau khi hợp nhất nhiều lần bất kỳ cặp nào bằng cách thay thế chúng bằng phép chia OR theo bit của chúng một cách tối ưu. Kết quả nổi tiếng là chiến lược tối ưu là hợp nhất nhiều lần các phần tử sao cho tập hợp cuối cùng trở thành một phần tử bằng OR toàn cục và tất cả các phần tử khác bằng 0, mang lại câu trả lời bằng OR + tổng các số 0 còn lại, là OR. 

Nhưng vì các số 0 vẫn phải tính đến các ràng buộc phân phối, nên bất biến đúng sẽ đơn giản hóa thành: câu trả lời bằng tổng các phần tử sau khi thay thế tất cả trừ một phần tử bằng 0 trong khi giữ nguyên tổng HOẶC, đánh giá bằng tổng trừ tổng của biểu diễn tối thiểu và điều này phân giải thành tổng (a) với hợp nhất bit tối ưu cho câu trả lời cuối cùng bằng tổng của tất cả các phần tử trừ đi tổng tiết kiệm bit trùng lặp, tương đương với tổng (a) khi không hợp nhất giúp ích, nhưng giảm nghiêm ngặt thành tổng đóng góp bit riêng lẻ sau khi loại bỏ trùng lặp. 

Một quan sát chính xác và khả thi hơn là mọi thao tác đều cho phép chúng ta chuyển bit giữa hai số một cách hiệu quả mà không thay đổi OR của chúng, nghĩa là chúng ta có thể kết hợp tất cả các bit thành một phần tử và tất cả các bit khác trở thành số 0. Do đó, tổng tối thiểu cuối cùng chính xác là OR của toàn bộ mảng. 

Do đó, vấn đề giảm xuống còn tính toán OR theo bit của tất cả các phần tử. 

### Bảng so sánh 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng trạng thái Brute Force | Hàm mũ | Hàm mũ | Quá chậm | 
| Tổng hợp theo bit HOẶC | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi trường hợp kiểm thử, hãy đọc mảng và tính toán OR theo bit của tất cả các phần tử. Điều này nắm bắt tất cả các bit phải tồn tại trong bất kỳ cấu hình cuối cùng hợp lệ nào vì không có thao tác nào có thể thêm hoặc loại bỏ các bit trên toàn cầu. 
2. Duy trì một biến đang chạy`result`được khởi tạo thành 0 và với mỗi lần cập nhật phần tử`result |= a[i]`. Điều này đảm bảo mọi bit có trong bất kỳ phần tử nào đều được giữ nguyên. 
3. Sau khi xử lý toàn bộ mảng, xuất ra`result`như câu trả lời cho trường hợp thử nghiệm đó. 

### Tại sao nó hoạt động 

Thuộc tính quan trọng là OR của toàn bộ mảng là bất biến trong phép toán được phép. Bất kỳ sự thay thế nào của hai số đều giữ nguyên cặp OR của chúng, do đó không có bit nào xuất hiện ở bất kỳ đâu có thể biến mất khỏi OR toàn cục. Ngược lại, mọi cấu hình cuối cùng hợp lệ vẫn phải bao gồm chính xác các bit đó. Vì việc giảm thiểu tổng theo các ràng buộc này tương đương với việc tập trung tất cả các bit cần thiết mà không bị trùng lặp, nên kết quả tốt nhất có thể đạt được tương ứng với việc biểu diễn tất cả các bit được yêu cầu một lần, chính xác là giá trị OR toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    arr = list(map(int, input().split()))
    
    res = 0
    for x in arr:
        res |= x
    
    print(res)
```Mã xử lý từng trường hợp thử nghiệm một cách độc lập. Các phần tử vòng lặp tích lũy OR theo bit, đây là giá trị duy nhất quan trọng cho câu trả lời cuối cùng. Không có trường hợp đặc biệt nào trong quá trình triển khai ngoài việc đảm bảo xử lý đầu vào nhanh, vì tất cả các giá trị đều vừa khít với số nguyên Python. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 3 2
```Chúng tôi theo dõi sự tích lũy OR. 

| Bước | Yếu tố | Hiện tại HOẶC | 
| --- | --- | --- | 
| 1 | 1 | 1 | 
| 2 | 3 | 3 | 
| 3 | 2 | 3 | 

Câu trả lời cuối cùng là 3. 

Điều này cho thấy các bit chồng chéo được hợp nhất thành một giá trị đại diện duy nhất như thế nào. 

### Ví dụ 2 

đầu vào:```
5
1 2 4 8 16
```| Bước | Yếu tố | Hiện tại HOẶC | 
| --- | --- | --- | 
| 1 | 1 | 1 | 
| 2 | 2 | 3 | 
| 3 | 4 | 7 | 
| 4 | 8 | 15 | 
| 5 | 16 | 31 | 

Câu trả lời cuối cùng là 31. 

Điều này xác nhận rằng khi các bit rời rạc, OR sẽ tích lũy tất cả các đóng góp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi phần tử được xử lý một lần bằng phép toán OR theo bit | 
| Không gian | O(1) | Chỉ sử dụng một bộ tích lũy duy nhất | 

Các ràng buộc cho phép tối đa 1000 trường hợp kiểm thử với các mảng có kích thước lên tới 100, do đó việc quét tuyến tính trên mỗi trường hợp kiểm thử dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        n = int(input())
        arr = list(map(int, input().split()))
        res = 0
        for x in arr:
            res |= x
        output.append(str(res))
    return "\n".join(output)

# provided samples
assert run("""4
3
1 3 2
5
1 2 4 8 16
2
6 6
3
3 5 6
""") == """3
31
6
7"""

# all equal
assert run("""1
4
7 7 7 7
""") == "7"

# minimum size
assert run("""1
2
1 2
""") == "3"

# single bit chain
assert run("""1
5
1 1 1 1 1
""") == "1"

# mixed bits
assert run("""1
3
2 4 6
""") == "6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 7 7 7 7 | 7 | dư thừa và các giá trị giống hệt nhau | 
| 1 2 | 3 | kích thước mảng hợp lệ nhỏ nhất | 
| 1 1 1 1 1 | 1 | các yếu tố giống hệt nhau lặp đi lặp lại | 
| 2 4 6 | 6 | mẫu bit chồng chéo | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các số đều giống hệt nhau. Ví dụ,`[7, 7, 7]`có OR bằng 7. Thuật toán xuất ra chính xác 7 và không có thao tác nào có thể giảm thêm nữa vì mọi phân phối lại vẫn giữ nguyên cấu trúc bit. 

Một trường hợp khác là khi các giá trị tách rời nhau theo cặp bit, chẳng hạn như`[1, 2, 4, 8]`. OR tích lũy thành 15 và thuật toán tạo ra 15. Điều này phù hợp với trực giác rằng không việc hợp nhất nào có thể làm giảm tổng chi phí bit vì mỗi phần tử đóng góp một bit duy nhất. 

Trường hợp cuối cùng là mảng kích thước tối thiểu như`[a, b]`. OR của cả hai luôn đúng và vì không có thao tác nào có thể giảm xuống dưới mức bảo toàn OR nên câu trả lời đơn giản là`a | b`.
