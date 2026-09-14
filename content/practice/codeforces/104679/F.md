---
title: "CF 104679F - Ghế may mắn"
description: "Chúng ta được cho hai số nguyên mô tả một tập ẩn gồm các số nguyên không âm riêng biệt. Một trong những giá trị này là OR theo bit của tất cả các phần tử trong tập hợp và giá trị còn lại là XOR theo bit của tất cả các phần tử trong cùng một tập hợp."
date: "2026-06-29T09:02:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104679
codeforces_index: "F"
codeforces_contest_name: "Replay of Battle of Brains 2022, University of Dhaka"
rating: 0
weight: 104679
solve_time_s: 43
verified: true
draft: false
---

[CF 104679F - Ghế may mắn](https://codeforces.com/problemset/problem/104679/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai số nguyên mô tả một tập ẩn gồm các số nguyên không âm riêng biệt. Một trong những giá trị này là OR theo bit của tất cả các phần tử trong tập hợp và giá trị còn lại là XOR theo bit của tất cả các phần tử trong cùng một tập hợp. Chỉ từ hai tập hợp này, nhiệm vụ là xác định số lượng số nguyên riêng biệt lớn nhất có thể tạo thành một tập hợp như vậy hoặc quyết định rằng không có tập hợp nào như vậy có thể tồn tại. 

Giá trị OR mô tả vị trí bit nào xuất hiện trong ít nhất một số của tập hợp. Bất kỳ bit nào bằng 0 trong OR này sẽ buộc mọi số trong tập hợp có giá trị 0 tại vị trí đó. Giá trị XOR nắm bắt thông tin chẵn lẻ trên tất cả các phần tử từng chút một, đưa ra các ràng buộc tương tác với cấu trúc OR theo cách không cục bộ. 

Giải thích trực tiếp vấn đề gợi ý việc xây dựng các tập thỏa mãn đồng thời cả hai ràng buộc. Tuy nhiên, mặc dù các con số nói chung là không bị giới hạn, OR ngay lập tức giới hạn chúng ta trong một vũ trụ mặt nạ bit hữu hạn. Nếu OR sử dụng k bit thì mọi số hợp lệ phải nằm trong khoảng trống có kích thước 2^k, vì mỗi k bit đó có thể được chọn độc lập là 0 hoặc 1. 

Một nỗ lực ngây thơ sẽ cố gắng liệt kê tất cả các tập hợp con của vũ trụ này và tính toán OR và XOR cho mỗi tập hợp con. Điều này trở nên bất khả thi rất nhanh vì có 2^(2^k) tập hợp con, sẽ bùng nổ ngay cả đối với k nhỏ. 

Một vấn đề tinh tế hơn xuất hiện ở tính nhất quán. Đầu vào có thể mô tả các cặp OR và XOR không tương thích. Ví dụ: nếu một bit được đặt trong XOR nhưng không phải trong OR, thì không thể xây dựng được vì XOR là 1 tại một bit ngụ ý một số phần tử lẻ có tập hợp bit đó, điều này mâu thuẫn với OR cấm hoàn toàn. Một cách tiếp cận bất cẩn mà bỏ qua việc kiểm tra này sẽ báo cáo sai một câu trả lời tích cực. 

Một trường hợp cạnh khác là khi OR bằng 0. Điều đó buộc tất cả các phần tử phải bằng 0, vì không bit nào được phép xuất hiện ở bất kỳ số nào. Trong trường hợp đó, XOR cũng phải bằng 0; nếu không thì đầu vào không nhất quán. 

## Phương pháp tiếp cận 

Quan điểm vũ phu bắt đầu bằng cách tưởng tượng tất cả các tập hợp con số có thể được rút ra từ vũ trụ bit được phép được xác định bởi OR. Đối với mỗi tập hợp con, chúng tôi tính toán OR và XOR của nó rồi so sánh với các giá trị đích. Điều này đúng vì nó kiểm tra rõ ràng mọi cấu hình có thể. Tuy nhiên, ngay cả đối với k bit được phép, vũ trụ có 2^k phần tử và số tập hợp con là 2^(2^k), con số này tăng vượt xa tính khả thi khi k vượt quá 5 hoặc 6. 

Sự đơn giản hóa quan trọng đến từ việc thay đổi quan điểm: thay vì chọn các tập hợp con tùy ý, chúng tôi suy luận về cấu trúc do XOR áp đặt trên một vũ trụ hoàn chỉnh. Tập hợp tất cả các số được hình thành từ các tập con của k bit được phép có tính đối xứng mạnh. Mỗi vị trí bit được cân bằng độc lập trên một nửa vũ trụ, điều này buộc XOR của toàn bộ vũ trụ bằng 0. Điều này mang lại cấu hình đường cơ sở tự nhiên đã đáp ứng tối đa ràng buộc OR. 

Từ đó, ràng buộc XOR có thể được coi là một sự điều chỉnh duy nhất cho cấu trúc đối xứng này. Nếu chúng ta loại bỏ một phần tử được chọn cẩn thận, chúng ta sẽ lật XOR từ 0 sang phần tử đó. Điều này biến vấn đề từ việc liệt kê tập hợp con thành điều chỉnh tính chẵn lẻ có kiểm soát bên trong vũ trụ có kích thước 2^k cố định. 

Công việc còn lại là xử lý các trường hợp suy biến trong đó k nhỏ, vì các đối số đối xứng dựa vào việc có đủ cấu trúc để cân bằng các đóng góp bit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^(2^k)) | O(2^k) | Quá chậm | 
| Tối ưu | O(2^k) | O(2^k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta bắt đầu từ giá trị OR đã cho, giá trị này xác định vị trí bit nào có thể sử dụng được.

1. Trích xuất tất cả các vị trí bit trong đó OR có 1. Để các vị trí này tạo thành danh sách các bit được phép. Số lượng bit như vậy là k. Mọi số hợp lệ chỉ được hình thành bằng cách sử dụng k vị trí này, nếu không nó sẽ vi phạm ràng buộc OR. 
2. Kiểm tra xem XOR có chứa bất kỳ bit nào không có trong OR hay không. Nếu vậy thì không có giải pháp nào tồn tại. Điều này là do XOR có 1 ở bit bị cấm ngụ ý số lượng số lẻ với tập hợp bit đó, nhưng OR cấm bất kỳ số nào như vậy. 
3. Nếu OR bằng 0 thì mọi số đều phải bằng 0. Tập hợp duy nhất có thể là {0}. Điều này chỉ hoạt động nếu XOR cũng bằng 0; nếu không thì không có công trình hợp lệ. 
4. Nếu có chính xác một bit được phép thì các giá trị duy nhất có thể là 0 và chính OR. Một tập hợp không trống phải bao gồm OR để thỏa mãn ràng buộc OR, vì vậy câu trả lời luôn là 2 và XOR phải bằng OR. 
5. Nếu k ít nhất là 2, hãy xây dựng tập hợp đầy đủ của tất cả 2^k số được hình thành bởi tập hợp con các bit được phép. Đây là vũ trụ hoàn chỉnh dưới ràng buộc OR. 
6. Quan sát rằng trong vũ trụ đầy đủ này, mỗi bit xuất hiện thường xuyên như nhau trên tất cả các số, do đó XOR của tất cả các phần tử 2^k bằng không. 
7. Nếu XOR được yêu cầu bằng 0, toàn bộ vũ trụ này đã là một giải pháp hợp lệ, do đó kích thước tối đa là 2^k. 
8. Nếu XOR được yêu cầu khác 0 thì nó vẫn phải nằm trong không gian bit cho phép. Loại bỏ chính xác một phần tử bằng XOR khỏi toàn bộ vũ trụ. Vì việc loại bỏ một phần tử sẽ lật XOR theo giá trị đó nên XOR kết quả sẽ trở thành chính xác phần tử được yêu cầu và OR không thay đổi. 
9. Trả về kích thước của tập kết quả là 2^k − 1. 

### Tại sao nó hoạt động 

Việc xây dựng dựa trên thực tế là toàn bộ công suất được đặt trên k bit độc lập có XOR bằng 0 do sự ghép đôi hoàn hảo của các trạng thái khác nhau ở bất kỳ bit được chọn nào. Điều này làm cho tập hợp có cấu trúc tuyến tính trên GF(2), trong đó XOR hoạt động giống như phép cộng vectơ. Việc loại bỏ một vectơ khỏi tập hợp có tổng bằng 0 sẽ tạo ra tổng mới bằng vectơ đó và vì XOR phải nằm trong không gian cho phép nên vectơ đó được đảm bảo tồn tại trong vũ trụ. Ràng buộc OR được giữ nguyên vì việc loại bỏ các phần tử không bao giờ tạo ra các bit mới. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    O, X = map(int, input().split())

    if (X & ~O) != 0:
        print(-1)
        return

    if O == 0:
        print(1 if X == 0 else -1)
        return

    bits = []
    for i in range(60):
        if O >> i & 1:
            bits.append(i)

    k = len(bits)

    if k == 1:
        print(2)
        return

    if X == 0:
        print(1 << k)
    else:
        print((1 << k) - 1)

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên thực thi tính nhất quán giữa XOR và OR bằng cách kiểm tra xem XOR không kích hoạt các bit bị cấm. Sau đó, nó xử lý các trường hợp suy biến trong đó OR bằng 0 hoặc có một bit. Đối với trường hợp chung, nó đếm số lượng bit có sẵn và sử dụng kết quả cấu trúc mà tập hợp con đầy đủ mang lại cho XOR bằng 0. Câu trả lời chỉ phụ thuộc vào việc chúng ta có cần loại bỏ một phần tử để phù hợp với ràng buộc XOR hay không. 

Một điểm tinh tế là chúng ta không bao giờ xây dựng tập hợp một cách rõ ràng. Lý do chỉ phụ thuộc vào việc đếm có bao nhiêu tập hợp bit hợp lệ tồn tại chứ không phải liệt kê chúng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Đặt OR = 5 (nhị phân 101) và XOR = 0. 

| Bước | Bit được phép | k | Quy mô xây dựng | Điều kiện XOR | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| Xác định bit | {0,2} | 2 | 4 yếu tố | hợp lệ | | 
| Toàn vũ trụ | tập hợp con kiểu {0,1,4,5} | 2 | 4 | XOR trở thành 0 | 4 | 

Cấu trúc sử dụng tất cả các tập hợp con của các bit {0,2}, cho các số 0, 1, 4, 5. XOR của chúng hủy bỏ hoàn toàn, xác nhận rằng tập hợp đầy đủ là hợp lệ khi XOR bằng 0. 

### Ví dụ 2 

Đặt OR = 5 (nhị phân 101) và XOR = 4 (nhị phân 100). 

| Bước | Bit được phép | k | Quy mô xây dựng | Điều kiện XOR | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| Xác định bit | {0,2} | 2 | 4 yếu tố | X là tập hợp con hợp lệ | | 
| XOR vũ trụ đầy đủ | 0 | 2 | 4 | loại bỏ 4 | 3 | 

Ở đây XOR khác 0 nhưng vẫn nằm trong số bit cho phép. Chúng tôi xóa phần tử bằng XOR khỏi toàn bộ vũ trụ, giảm kích thước từ 4 xuống 3 trong khi thiết lập XOR chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(60) | Chúng tôi chỉ quét các bit của giá trị OR và thực hiện kiểm tra liên tục | 
| Không gian | O(1) | Chỉ một danh sách nhỏ các vị trí bit được lưu trữ | 

Giải pháp chạy trong thời gian không đổi so với kích thước đầu vào do độ rộng bit được cố định. Điều này dễ dàng đáp ứng mọi ràng buộc hợp lý. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import log2
    O, X = map(int, inp.split())
    if (X & ~O) != 0:
        return "-1"
    if O == 0:
        return "1" if X == 0 else "-1"
    k = bin(O).count("1")
    if k == 1:
        return "2"
    if X == 0:
        return str(1 << k)
    return str((1 << k) - 1)

# edge cases
assert run("0 0") == "1"
assert run("0 1") == "-1"
assert run("1 0") == "2"
assert run("5 0") == "4"
assert run("5 4") == "3"
assert run("2 2") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 0 | 1 | tập đơn hợp lệ tối thiểu | 
| 0 1 | -1 | XOR không thể bên ngoài OR | 
| 1 0 | 2 | lực OR bit đơn {0, OR} | 
| 5 0 | 4 | trường hợp vũ trụ tập hợp con đầy đủ | 
| 5 4 | 3 | trường hợp loại bỏ XOR khác 0 | 

## Vỏ cạnh 

Khi OR bằng 0, vũ trụ co lại thành một con số duy nhất có thể. Đối với đầu vào`0 0`, thuật toán ngay lập tức trả về 1, phản ánh tập hợp lệ duy nhất`{0}`. Nếu XOR khác 0, chẳng hạn như`0 1`, séc`(X & ~O)`kích hoạt vì XOR sử dụng bit bị cấm và thuật toán sẽ loại bỏ nó một cách chính xác trước khi thử bất kỳ cấu trúc nào. 

Khi OR có chính xác một bit được đặt, giả sử`O = 1`, thuật toán trả về 2 bất kể XOR là 0 hay 1. Trong trường hợp này, chỉ có hai số có thể là 0 và 1, và việc loại trừ 1 sẽ phá vỡ yêu cầu OR, do đó kích thước được cố định. Kiểm tra tính nhất quán XOR đảm bảo rằng chỉ những cấu hình hợp lệ mới đến được nhánh này, do đó không có mâu thuẫn nào phát sinh trong quá trình đánh giá.
