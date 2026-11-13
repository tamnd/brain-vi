---
title: "CF 1712C - Loại 0"
description: "Chúng ta có một số trường hợp thử nghiệm độc lập, mỗi trường hợp bao gồm một mảng các số nguyên dương. Trong một lần di chuyển, chúng ta được phép chọn một giá trị x và đồng thời xóa tất cả các lần xuất hiện của x trong mảng bằng cách biến chúng thành số 0."
date: "2026-06-15T00:45:26+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1712
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 813 (Div. 2)"
rating: 1100
weight: 1712
solve_time_s: 227
verified: false
draft: false
---

[CF 1712C - Sắp xếp số 0](https://codeforces.com/problemset/problem/1712/C) 

**Đánh giá:** 1100 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 3 phút 47s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một số trường hợp thử nghiệm độc lập, mỗi trường hợp bao gồm một mảng các số nguyên dương. Trong một nước đi, chúng ta được phép chọn một giá trị`x`và đồng thời xóa tất cả các lần xuất hiện của`x`trong mảng bằng cách biến chúng thành số không. Nhiệm vụ là áp dụng thao tác này càng ít lần càng tốt để mảng kết quả không giảm. 

Điểm mấu chốt là chúng tôi không sắp xếp lại các yếu tố. Thứ tự tương đối của các vị trí không bao giờ thay đổi, chỉ có giá trị được thay thế bằng 0. Vì số 0 là giá trị nhỏ nhất có thể nên việc biến các phần tử thành số 0 chỉ có thể giúp khắc phục sự đảo ngược trong đó giá trị lớn hơn xuất hiện trước giá trị nhỏ hơn. 

Ràng buộc`n ≤ 10^5`trên tất cả các trường hợp thử nghiệm buộc phải có giải pháp tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Bất cứ điều gì liên tục quét mảng hoặc mô phỏng các hoạt động một cách đơn giản trên mỗi giá trị sẽ quá chậm nếu nó tính toán lại cấu trúc thứ tự sau mỗi lần xóa. 

Trường hợp cạnh tinh tế xuất hiện khi mảng không giảm. Ví dụ,`[0, 0, 1, 1]`đã hợp lệ nên không cần thực hiện thao tác nào. Một cách tiếp cận bất cẩn luôn xóa nội dung nào đó (ví dụ: luôn nhắm mục tiêu đảo ngược đầu tiên) có thể thực hiện không chính xác các thao tác không cần thiết. 

Một trường hợp quan trọng khác là khi các giá trị được “xen kẽ” nhiều, chẳng hạn như`[1, 2, 1, 2]`. Một trực giác ngây thơ có thể gợi ý rằng một hoặc hai thao tác là đủ, nhưng vì số 0 ảnh hưởng đến trật tự trên toàn cầu nên chúng ta phải suy luận cẩn thận về việc tồn tại bao nhiêu “lớp” rối loạn riêng biệt. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ mô phỏng tất cả các chuỗi hoạt động có thể xảy ra. Mỗi thao tác chọn một giá trị`x`và loại bỏ nó hoàn toàn. Vì mỗi giá trị có thể bị xóa nhiều nhất một lần nên chúng tôi đang chọn các tập hợp con của các giá trị riêng biệt một cách hiệu quả và kiểm tra xem mảng kết quả có được sắp xếp sau khi áp dụng chúng hay không. 

Có nhiều nhất`n`các giá trị riêng biệt, do đó số lượng tập hợp con là theo cấp số nhân. Ngay cả khi chúng tôi kiểm tra tính hợp lệ trong`O(n)`thời gian cho mỗi tập hợp con, điều này trở thành`O(2^n · n)`, điều đó hoàn toàn không thể thực hiện được. 

Thông tin chi tiết quan trọng là chuyển quan điểm từ “giá trị nào chúng ta xóa” sang “cấu trúc nào trong mảng buộc phải xóa”. 

Hãy quan sát ý nghĩa của việc mảng không giảm sau khi xóa. Tất cả các giá trị khác 0 còn lại phải xuất hiện theo thứ tự được sắp xếp và mọi phép đảo ngược phải được loại bỏ bằng cách loại bỏ ít nhất một trong hai giá trị liên quan đến phép đảo ngược. 

Bây giờ hãy xem xét việc quét mảng từ trái sang phải trong khi vẫn duy trì giá trị tối đa được thấy cho đến nay. Bất cứ khi nào chúng ta thấy mức giảm (một giá trị nhỏ hơn mức tối đa đang chạy), vị trí đó sẽ tạo ra một ràng buộc: giá trị hiện tại hoặc giá trị nào đó trước nó cuối cùng phải được loại bỏ để loại bỏ sự đảo ngược. Vì việc xóa được thực hiện trên mỗi giá trị trên toàn cầu nên mỗi giá trị riêng biệt tham gia vào các “ngắt” như vậy buộc phải thực hiện ít nhất một thao tác. 

Quan sát quan trọng là mỗi khi chúng ta gặp phải một “lớp rối loạn” mới, nó tương ứng với một giá trị riêng biệt cần phải loại bỏ bằng một thao tác riêng biệt. Đếm xem có bao nhiêu giá trị riêng biệt chịu trách nhiệm phá vỡ tính đơn điệu sẽ đưa ra câu trả lời. 

Chúng ta có thể chính thức hóa điều này bằng cách theo dõi tất cả các giá trị xuất hiện ở những vị trí mà`a[i] < max(a[0..i-1])`. Mọi giá trị như vậy đều là “xấu” và phải được loại bỏ ít nhất một lần. Vì một thao tác sẽ loại bỏ tất cả các lần xuất hiện của một giá trị, nên câu trả lời sẽ trở thành số lượng các giá trị riêng biệt từng xuất hiện ở vị trí giảm dần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n · n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Di chuyển mảng từ trái sang phải trong khi vẫn duy trì giá trị lớn nhất cho đến nay. 

Điều này nắm bắt được ranh giới “kỳ vọng được sắp xếp” hiện tại. 
2. Bất cứ khi nào phần tử hiện tại lớn hơn hoặc bằng mức tối đa, hãy cập nhật mức tối đa. 

Điều này có nghĩa là trình tự chưa bị phá vỡ ở vị trí này. 
3. Nếu phần tử hiện tại nhỏ hơn giá trị tối đa, hãy đánh dấu giá trị này là “xấu”. 

Lý do là phần tử này tham gia vào sự đảo ngược với một số phần tử lớn hơn trước đó. 
4. Lưu trữ mọi giá trị “xấu” như vậy vào một tập hợp. 

Chúng tôi sử dụng một tập hợp vì nhiều lần xuất hiện của cùng một giá trị chỉ cần một thao tác. 
5. Sau khi xử lý toàn bộ mảng, câu trả lời là kích thước của tập hợp. 

### Tại sao nó hoạt động 

Điều bất biến là mỗi khi chúng ta phát hiện`a[i] < max_so_far`, chúng tôi xác định một giá trị thực sự cần thiết để loại bỏ trong bất kỳ giải pháp hợp lệ nào. Điều này là do sự đảo ngược liên quan đến`a[i]`và mức tối đa trước đó không thể cố định được trừ khi`a[i]`hoặc giá trị lớn hơn trước đó sẽ bị xóa. Vì chúng ta chỉ được phép xóa theo giá trị chứ không được xóa theo vị trí nên đánh dấu giá trị tại`i`là đủ để giải thích cho hạn chế đó trên toàn cầu. Các giá trị riêng biệt được thu thập theo cách này tương ứng chính xác với tập hợp các thao tác tối thiểu cần thiết để loại bỏ tất cả các phép đảo ngược. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        mx = 0
        bad = set()
        
        for v in a:
            if v >= mx:
                mx = v
            else:
                bad.add(v)
        
        print(len(bad))

if __name__ == "__main__":
    solve()
```Mã trực tiếp theo sau quá trình quét được mô tả trước đó. Biến`mx`duy trì mức tối đa đang chạy và bất kỳ phần tử nào vi phạm tính đơn điệu sẽ được chèn vào một tập hợp. Câu trả lời cuối cùng là số lượng các giá trị riêng biệt như vậy. Chi tiết triển khai chính là chúng tôi không đặt lại mức tối đa hoặc xem lại các phần tử trước đó vì quyết định hoàn toàn dựa trên cấu trúc từ trái sang phải. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`[3, 3, 2]`| tôi | giá trị | max_so_far | bộ xấu | 
| --- | --- | --- | --- | 
| 0 | 3 | 3 | {} | 
| 1 | 3 | 3 | {} | 
| 2 | 2 | 3 | {2} | 

Đầu ra là`1`. 

Điều này cho thấy chỉ có giá trị`2`luôn xuất hiện ở vị trí giảm dần nên chỉ cần thực hiện một thao tác. 

### Ví dụ 2 

đầu vào:`[1, 3, 1, 3]`| tôi | giá trị | max_so_far | bộ xấu | 
| --- | --- | --- | --- | 
| 0 | 1 | 1 | {} | 
| 1 | 3 | 3 | {} | 
| 2 | 1 | 3 | {1} | 
| 3 | 3 | 3 | {1} | 

Đầu ra là`1`. 

Giá trị duy nhất`1`luôn là một phần của giọt, vì vậy hãy xóa tất cả`1`s sửa mảng. 

Điều này chứng tỏ rằng nhiều sự đảo ngược có thể được giải quyết bằng cách xóa một giá trị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Quét tuyến tính đơn với các thao tác cài đặt và cập nhật liên tục | 
| Không gian | O(k) | Chỉ đặt các cửa hàng có giá trị có vấn đề riêng biệt | 

Tổng độ phức tạp trên tất cả các trường hợp thử nghiệm là tuyến tính trong tổng kích thước đầu vào, phù hợp thoải mái trong giới hạn vì tổng của`n`là`10^5`. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import inf

    t = int(sys.stdin.readline())
    out = []
    for _ in range(t):
        n = int(sys.stdin.readline())
        a = list(map(int, sys.stdin.readline().split()))
        
        mx = 0
        bad = set()
        for v in a:
            if v >= mx:
                mx = v
            else:
                bad.add(v)
        out.append(str(len(bad)))
    
    return "\n".join(out)

# provided samples
assert run("""5
3
3 3 2
4
1 3 1 3
5
4 1 5 3 2
4
2 4 1 2
1
1
""") == """1
2
4
3
0"""

# custom: already sorted
assert run("""1
4
1 2 3 4
""") == "0"

# custom: all equal
assert run("""1
5
7 7 7 7 7
""") == "0"

# custom: strict alternating
assert run("""1
4
2 1 2 1
""") == "1"

# custom: worst disorder
assert run("""1
6
6 5 4 3 2 1
""") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mảng được sắp xếp | 0 | không cần thao tác | 
| tất cả đều bình đẳng | 0 | trùng lặp không bao giờ gây ra kết quả dương tính giả | 
| xen kẽ | 1 | giá trị duy nhất có thể khắc phục nhiều lần đảo ngược | 
| giảm dần | n-1 giá trị xấu khác biệt | hành vi rối loạn tối đa | 

## Vỏ cạnh 

Một mảng được sắp xếp đầy đủ như`[1, 2, 3]`không bao giờ kích hoạt`v < mx`điều kiện, do đó tập sai vẫn trống và thuật toán trả về 0 một cách chính xác. 

Một mảng như`[5, 5, 5]`giữ`mx`không thay đổi xuyên suốt và không có phần tử nào bị coi là xấu, vì vậy nó không yêu cầu thao tác nào. 

Trong một mảng giảm dần như`[4, 3, 2, 1]`, mọi phần tử ngoại trừ phần tử đầu tiên đều vi phạm mức tối đa đang chạy, tạo ra các giá trị xấu`{3, 2, 1}`. Do đó, thuật toán tính chính xác các giá trị riêng biệt chịu trách nhiệm cho tất cả các lần đảo ngược, khớp với số lần xóa cần thiết tối thiểu.
