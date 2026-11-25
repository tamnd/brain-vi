---
title: "CF 988C - Tổng bằng nhau"
description: "Chúng tôi được cung cấp một số mảng số nguyên độc lập. Từ mỗi mảng, chúng ta được phép loại bỏ chính xác một phần tử và điều này tạo ra một “tổng đã sửa đổi” cho mảng đó, nghĩa là tổng ban đầu trừ đi phần tử bị loại bỏ."
date: "2026-06-17T00:47:39+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 988
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 486 (Div. 3)"
rating: 1400
weight: 988
solve_time_s: 83
verified: false
draft: false
---

[CF 988C - Tổng bằng nhau](https://codeforces.com/problemset/problem/988/C) 

**Đánh giá:** 1400 
**Tags:** triển khai, sắp xếp 
**Thời gian giải:** 1 phút 23s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số mảng số nguyên độc lập. Từ mỗi mảng, chúng ta được phép loại bỏ chính xác một phần tử và điều này tạo ra một “tổng đã sửa đổi” cho mảng đó, nghĩa là tổng ban đầu trừ đi phần tử bị loại bỏ. Nhiệm vụ là chọn hai mảng khác nhau và chọn một vị trí loại bỏ trong mỗi mảng sao cho tổng được sửa đổi thu được trở nên bằng nhau. 

Diễn đạt lại điều này theo cách có cấu trúc hơn, đối với mỗi chuỗi, chúng ta có thể tính tổng của nó và mọi phần tử trong đó xác định một giá trị ứng cử viên thu được bằng cách trừ phần tử đó khỏi tổng số. Vấn đề giảm xuống còn việc tìm hai chuỗi có thể tạo ra cùng một giá trị ứng viên như vậy. 

Các ràng buộc chặt chẽ theo một cách rất cụ thể. Tổng số phần tử trên tất cả các chuỗi tối đa là 200.000 và có thể lên tới 200.000 chuỗi. Bất kỳ giải pháp nào cố gắng so sánh trực tiếp từng cặp chuỗi sẽ yêu cầu hành vi bậc hai về số lượng chuỗi, vượt xa giới hạn khả thi. Ngay cả các phép toán bậc hai trên mỗi chuỗi cũng không được phép vì tổng chiều dài lớn. Điều này thúc đẩy chúng ta hướng tới chiến lược tổng hợp tuyến tính hoặc gần tuyến tính trên tất cả các phần tử. 

Một cạm bẫy ngây thơ xuất phát từ việc coi vấn đề là “khớp hai chuỗi bằng cách so sánh tất cả các lần loại bỏ có thể một cách độc lập”. Ví dụ: nếu chúng tôi tạo tất cả các tổng sau khi xóa cho mỗi chuỗi và sau đó cố gắng khớp chúng trên toàn cầu, chúng tôi có thể quên rằng cùng một tổng mục tiêu có thể xuất hiện nhiều lần trong một chuỗi và chúng tôi phải đảm bảo hai lần loại bỏ đã chọn đến từ các chuỗi khác nhau. 

Một trường hợp lỗi tinh vi khác xảy ra nếu chúng ta chỉ lưu trữ loại bỏ tốt nhất trên mỗi chuỗi hoặc chỉ xem xét các tổng duy nhất trên mỗi chuỗi. Một trình tự như`[1, 1, 1]`tạo ra cùng một tổng ứng viên nhiều lần và việc bỏ qua bội số có thể loại bỏ các kết quả trùng khớp hợp lệ. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: đối với mỗi chuỗi, hãy thử loại bỏ từng phần tử và tính tổng kết quả, sau đó so sánh tất cả các cặp chuỗi và tất cả khả năng loại bỏ chúng. Nếu chúng ta biểu thị tổng số phần tử bằng N, thì mỗi chuỗi đóng góp O(n_i) ứng cử viên, do đó về tổng thể có các ứng cử viên O(N), nhưng việc ghép chúng giữa các chuỗi sẽ dẫn đến so sánh O(N^2) trong trường hợp xấu nhất. 

Điều này hoạt động về mặt khái niệm vì mọi câu trả lời hợp lệ phải xuất hiện ở đâu đó trong tập hợp đầy đủ các giá trị "tổng sau khi loại bỏ một phần tử". Điểm thất bại là tỷ lệ: với 200.000 phần tử, phép so sánh bậc hai tạo ra theo thứ tự 10^10 phép tính. 

Quan sát quan trọng là mỗi ứng cử viên được xác định đầy đủ bởi hai giá trị: tổng dãy S và phần tử a bị loại bỏ, tạo ra giá trị S − a. Thay vì mở rộng tất cả các ứng viên, chúng ta có thể xử lý từng phần tử một và cố gắng phát hiện sự xung đột của các giá trị này trên toàn cầu. 

Chúng tôi duy trì một bản đồ băm từ “tổng mục tiêu sau khi loại bỏ” đến cặp đầu tiên (chỉ mục chuỗi, chỉ mục phần tử) tạo ra nó. Ngay khi lần xuất hiện thứ hai của cùng một mục tiêu xuất hiện từ một chuỗi khác, chúng ta ngay lập tức có câu trả lời hợp lệ. 

Điều này biến vấn đề thành một lần chuyển qua tất cả các phần tử với phép băm theo thời gian không đổi cho mỗi phần tử. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N^2) | O(N) | Quá chậm | 
| Tối ưu | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi mọi phần tử như xác định “tổng kết quả” tiềm năng nếu nó bị xóa. 

### bước 

1. Tính tổng của mỗi dãy. 

Điều này là bắt buộc vì mọi giá trị ứng cử viên đều phụ thuộc vào việc trừ một phần tử khỏi tổng chuỗi của nó. 
2. Với mọi dãy i và mọi phần tử a tại vị trí x, tính giá trị: 

S_i − a. 

Điều này thể hiện số tiền còn lại sau khi loại bỏ phần tử đó. 
3. Duy trì bản đồ băm trong đó khóa là giá trị được tính toán này và giá trị lưu trữ một cặp (i, x) mô tả chuỗi nào và chỉ mục nào tạo ra nó đầu tiên. 

Chúng tôi chỉ lưu trữ lần xuất hiện đầu tiên vì chúng tôi chỉ cần phát hiện xung đột. 
4. Trong khi lặp lại, nếu chúng tôi tính toán một giá trị đã tồn tại trên bản đồ và nó đến từ một chuỗi khác, chúng tôi sẽ ngay lập tức trả về cả hai vị trí được lưu trữ. 

Điều này đảm bảo rằng chúng tôi đã tìm thấy hai chuỗi khác nhau tạo ra tổng số giảm như nhau. 
5. Nếu không tìm thấy xung đột sau khi xử lý tất cả các phần tử, xuất ra NO. 

### Tại sao nó hoạt động 

Mọi nghiệm hợp lệ đều tương ứng với một giá trị V sao cho tồn tại hai dãy i và j và các phần tử x thuộc i và y thuộc j thỏa mãn S_i − a[i][x] = S_j − a[j][y]. Bản đồ của chúng tôi lưu trữ mọi cặp nhìn thấy (V → lần xuất hiện đầu tiên). Lần đầu tiên một V trùng lặp xuất hiện từ một chuỗi khác, chúng ta có chính xác đẳng thức cần thiết. Vì chúng tôi xử lý tất cả các lần xóa có thể xảy ra chính xác một lần nên không thể bỏ sót câu trả lời hợp lệ nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    k = int(input())
    
    seq = []
    total = []
    
    for i in range(k):
        n = int(input())
        arr = list(map(int, input().split()))
        seq.append(arr)
        total.append(sum(arr))
    
    seen = {}
    
    for i in range(k):
        arr = seq[i]
        S = total[i]
        for j, val in enumerate(arr):
            key = S - val
            if key in seen:
                pi, pj = seen[key]
                if pi != i:
                    print("YES")
                    print(pi + 1, pj + 1)
                    print(i + 1, j + 1)
                    return
            else:
                seen[key] = (i, j)
    
    print("NO")

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách đọc tất cả các chuỗi và tính toán trước tổng của chúng, vì việc tính tổng lặp đi lặp lại sẽ làm tăng độ phức tạp. Logic chính sau đó lặp lại từng phần tử một lần, tính toán tổng rút gọn ứng cử viên. 

Bản đồ băm`seen`lưu trữ lần xuất hiện đầu tiên của mỗi giá trị ứng cử viên. Khi một bản sao xuất hiện từ một chuỗi khác, chúng tôi sẽ xuất ngay hai vị trí đó. Việc kiểm tra chỉ mục đảm bảo chúng tôi không bao giờ khớp một phần tử với một phần tử khác trong cùng một chuỗi, điều này sẽ vi phạm yêu cầu chọn hai chuỗi riêng biệt. 

Một chi tiết triển khai tinh tế là lưu trữ nội bộ các chỉ mục dựa trên 0 nhưng in các chỉ mục dựa trên một, vì vấn đề mong đợi đầu ra dựa trên 1. 

## Ví dụ đã hoạt động 

### Dấu vết ví dụ 

đầu vào:```
2
3
1 2 3
2
3 3
```| tôi | j | giá trị | khóa = S - giá trị | trạng thái nhìn thấy | hành động | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | 5 | {5:(0,0)} | cửa hàng | 
| 0 | 1 | 2 | 4 | {5:(0,0),4:(0,1)} | cửa hàng | 
| 0 | 2 | 3 | 3 | {5,4,3} | cửa hàng | 
| 1 | 0 | 3 | 3 | va chạm với (0,2) | trả lời | 

Chúng tôi phát hiện rằng việc loại bỏ 3 khỏi cả hai chuỗi sẽ mang lại số tiền còn lại bằng nhau. 

Điều này xác nhận tính bất biến rằng các khóa giống hệt nhau tương ứng chính xác với “tổng sau khi loại bỏ” bằng nhau. 

### Ví dụ thứ hai 

đầu vào:```
3
2
1 5
2
2 4
2
3 3
```| tôi | j | giá trị | chìa khóa | trạng thái nhìn thấy | hành động | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | 5 | {(5):(0,0)} | cửa hàng | 
| 0 | 1 | 5 | 1 | {(5):(0,0),(1):(0,1)} | cửa hàng | 
| 1 | 0 | 2 | 4 | ... | cửa hàng | 
| 1 | 1 | 4 | 2 | ... | cửa hàng | 
| 2 | 0 | 3 | 3 | ... | cửa hàng | 
| 2 | 1 | 3 | 3 | va chạm trong cùng một chuỗi bị bỏ qua | | 

Không xảy ra xung đột chuỗi chéo hợp lệ nên đầu ra là KHÔNG. 

Điều này cho thấy tại sao các va chạm cùng trình tự bị bỏ qua: chúng không thỏa mãn yêu cầu chọn hai mảng riêng biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi phần tử được xử lý một lần với các phép toán băm O(1) | 
| Không gian | O(N) | Bản đồ băm lưu trữ tối đa một mục nhập cho mỗi phần tử | 

Tổng số phần tử tối đa là 200.000, do đó, quét tuyến tính với hàm băm dễ dàng phù hợp với giới hạn thời gian. Việc sử dụng bộ nhớ cũng an toàn vì mỗi phần tử chỉ đóng góp một lượng siêu dữ liệu được lưu trữ không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample 1
assert run("""2
5
2 3 1 3 2
6
1 1 2 2 2 1
""") == """YES
2 6
1 2"""

# minimum case impossible
assert run("""2
1
5
1
7
""") == "NO"

# simple positive case
assert run("""2
2
1 2
2
2 1
""").startswith("YES")

# identical sequences
assert run("""2
3
1 1 1
3
1 1 1
""").startswith("YES")

# larger mix
assert run("""3
2
1 5
2
2 4
2
3 3
""") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 chuỗi, phần tử đơn lẻ | KHÔNG | điều không thể tối thiểu | 
| cặp đối xứng | CÓ | tính đúng đắn cơ bản | 
| mảng giống hệt nhau | CÓ | xử lý giá trị lặp lại | 
| 3 chuỗi không khớp | KHÔNG | trường hợp lỗi nhiều chuỗi | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi nhiều phần tử trong cùng một chuỗi tạo ra các tổng ứng cử viên giống hệt nhau. Ví dụ,`[1, 1, 1]`tạo ra cùng một số tiền giảm ba lần. Thuật toán chỉ lưu trữ lần xuất hiện đầu tiên nhưng vẫn xử lý các lần lặp lại sau đó, đảm bảo chúng có thể khớp với các chuỗi khác nếu cần. 

Một trường hợp khác là khi va chạm duy nhất xảy ra trong một chuỗi duy nhất. Coi như`[2,2]`Và`[1,3]`. Giá trị 2 xuất hiện hai lần trong chuỗi đầu tiên, nhưng điều đó không tạo thành câu trả lời hợp lệ. Séc`pi != i`ngăn ngừa dương tính giả. 

Trường hợp cuối cùng là khi chuỗi lớn nhưng không có kết quả trùng khớp. Thuật toán vẫn thực hiện quét tuyến tính đầy đủ mà không cần giả định sớm và bản đồ băm vẫn bị giới hạn bởi O(N), đảm bảo hành vi có thể dự đoán được ngay cả trong trường hợp đầu vào trong trường hợp xấu nhất.
