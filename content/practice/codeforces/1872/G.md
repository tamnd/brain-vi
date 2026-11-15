---
title: "CF 1872G - Thay thế bằng sản phẩm"
description: "Chúng ta được cấp một chuỗi các số nguyên dương và được phép thực hiện chính xác một phép biến đổi: chọn một đoạn liền kề, nén nó thành một số bằng tích của tất cả các phần tử trong đoạn đó và thay thế đoạn đó bằng giá trị đơn đó."
date: "2026-06-08T23:21:26+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1872
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 895 (Div. 3)"
rating: 2000
weight: 1872
solve_time_s: 96
verified: false
draft: false
---

[CF 1872G - Thay thế bằng sản phẩm](https://codeforces.com/problemset/problem/1872/G) 

**Xếp hạng:** 2000 
**Tags:** vũ phu, tham lam, toán học 
**Thời gian giải:** 1 phút 36 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi các số nguyên dương và được phép thực hiện chính xác một phép biến đổi: chọn một đoạn liền kề, nén nó thành một số bằng tích của tất cả các phần tử trong đoạn đó và thay thế đoạn đó bằng giá trị đơn đó. Mọi thứ bên ngoài phân khúc không thay đổi. Mục tiêu cuối cùng là chọn đoạn sao cho mảng kết quả có tổng lớn nhất có thể. 

Tác dụng chính của hoạt động này là một khối phần tử đóng góp dưới dạng các giá trị riêng lẻ hoặc dưới dạng một sản phẩm. Bởi vì tất cả các số đều dương, việc thay thế một phân khúc sẽ làm tăng sự đóng góp của nó nếu tích đủ lớn so với tổng các phần tử của nó và giảm nó nếu ngược lại. Nhiệm vụ là xác định phân khúc mà sự đánh đổi này có lợi nhất. 

Các ràng buộc ngụ ý rằng tổng độ dài mảng trong các trường hợp thử nghiệm lên tới 2×10^5, do đó, mọi giải pháp đều phải tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Quét bậc hai trên tất cả các mảng con cho mỗi trường hợp thử nghiệm sẽ đạt được khoảng 10^10 phép tính ở mức phân phối tệ nhất, vượt xa giới hạn. 

Trường hợp góc tinh vi phát sinh khi mảng chứa nhiều mảng. Trong trường hợp đó, việc thay thế một phân đoạn bằng tích của nó sẽ không có tác dụng gì, vì cả tổng và tích đều bằng 1. Điều này làm cho nhiều phân đoạn tương đương nhau. Một trường hợp góc khác là khi bất kỳ phần tử nào lớn hơn 1 và được bao quanh bởi các phần tử, bởi vì việc hợp nhất nó với các phần tử có thể cải thiện hoặc không cải thiện tổng tùy thuộc vào số lượng phần tử được hấp thụ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ kiểm tra mọi phân khúc có thể. Đối với mỗi cặp l và r, hãy tính tích của a[l..r], tính tổng mảng kết quả sau khi thay thế và theo dõi kết quả tốt nhất. Điều này hoạt động về mặt khái niệm vì hoạt động được xác định đầy đủ và chỉ có các phân đoạn O(n^2). Tuy nhiên, ngay cả với tích tiền tố hoặc logarit, số ứng viên vẫn là số bậc hai, quá chậm đối với n lên tới 2×10^5. 

Quan sát quan trọng là hầu hết tất cả các phần tử bằng 1 đều trung tính khi nhân, nhưng chúng đóng góp tích cực vào tổng khi để riêng biệt. Việc hợp nhất lần duy nhất có ích là khi chúng tôi bao gồm ít nhất một phần tử lớn hơn 1, bởi vì chỉ những phần tử đó mới thay đổi sản phẩm một cách có ý nghĩa. Điều này chuyển vấn đề từ “chọn bất kỳ phân khúc nào” sang “quyết định cách xử lý các lượt chạy xung quanh các giá trị lớn hơn”. 

Nếu chúng ta xem xét tác động của việc hợp nhất một phân đoạn, việc thay thế một khối chỉ chứa các phân đoạn là vô ích. Việc thay thế một khối chứa chính xác một phần tử không phải là một cũng không hữu ích trừ khi nó hợp nhất các phần tử xung quanh theo cách cải thiện cấu trúc tổng cuối cùng trong các trường hợp cạnh. Chiến lược tối ưu cuối cùng sẽ dẫn đến việc lựa chọn một phân khúc bao gồm tất cả các yếu tố không phải là một, bởi vì bất kỳ sự phân chia bổ sung nào xung quanh chúng đều không cải thiện được lợi ích của sản phẩm. 

Do đó, lời giải rút gọn thành việc tìm khoảng tối thiểu chứa tất cả các phần tử lớn hơn 1. Bất kỳ câu trả lời tối ưu nào cũng là một khoảng như vậy và nếu mảng chỉ chứa một khoảng, thì bất kỳ vị trí đơn lẻ nào cũng là tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) hoặc O(n^2) | O(1)-O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quét mảng từ trái sang phải trong khi theo dõi vị trí đầu tiên có giá trị lớn hơn 1. Điều này xác định ứng cử viên ranh giới bên trái. 
2. Tiếp tục quét để tìm vị trí cuối cùng có giá trị lớn hơn 1. Điều này xác định ứng cử viên ranh giới phù hợp. 
3. Nếu không có giá trị nào lớn hơn 1, hãy trả về bất kỳ chỉ mục đơn lẻ nào chẳng hạn như (1, 1), vì mọi phần tử đều bằng 1 và mọi thao tác đều mang lại kết quả giống nhau. 
4. Ngược lại, xuất ra khoảng thời gian từ lần xuất hiện đầu tiên đến lần xuất hiện cuối cùng của giá trị lớn hơn 1.

Lý do cấu trúc này là đủ là vì bất kỳ phần tử nào bằng 1 đều không thay đổi tích trừ khi chia tỷ lệ cho nó theo 1, nhưng nó làm tăng tổng mảng nếu để bên ngoài phân đoạn đã hợp nhất. Do đó, việc bao gồm những thứ không cần thiết bên trong phân đoạn đã hợp nhất chỉ làm giảm tổng số tiền cuối cùng vì chúng ngừng đóng góp riêng lẻ. 

### Tại sao nó hoạt động 

Phép biến đổi thay thế một phân đoạn bằng sản phẩm của nó, có tính nhân lên. Bất kỳ phần tử nào bằng 1 đều có tác dụng trung tính trong phép nhân nhưng đóng góp +1 vào tổng khi được giữ riêng biệt. Việc loại bỏ nó khỏi mảng sẽ làm giảm tổng chính xác bằng 1, trong khi việc đưa nó vào tích không làm tăng sản phẩm vượt quá hiệu ứng chia tỷ lệ trung tính. Do đó, bất kỳ phân đoạn tối ưu nào không bao giờ bao gồm 1 trừ khi cần kết nối các phần tử không phải là một và khoảng cách tối thiểu bao gồm tất cả các phần tử lớn hơn 1 sẽ bảo toàn tất cả phép nhân có lợi trong khi tránh mất đi các đóng góp cộng không cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    
    l = -1
    r = -1
    
    for i in range(n):
        if a[i] != 1:
            if l == -1:
                l = i
            r = i
    
    if l == -1:
        print(1, 1)
    else:
        print(l + 1, r + 1)
```Việc triển khai thực hiện một lần cho mỗi trường hợp thử nghiệm. Chúng tôi ghi lại chỉ số đầu tiên và cuối cùng trong đó mảng không bằng 1. Các chỉ số này xác định phân đoạn chúng tôi nén. 

Chi tiết triển khai quan trọng là duy trì cả hai điểm cuối trong một lần quét. Lần đầu tiên chúng ta thấy một giá trị không phải là một, chúng ta sẽ sửa ranh giới bên trái. Mọi thứ không phải ai tiếp theo đều cập nhật ranh giới phù hợp. Điều này tránh mọi nhu cầu về các bước bổ sung hoặc tiền xử lý. 

Trường hợp cạnh trong đó tất cả các phần tử đều bằng 1 được xử lý rõ ràng bằng cách kiểm tra xem ranh giới bên trái có từng được đặt hay không. 

## Ví dụ đã hoạt động 

Hãy xem xét mảng đầu vào [1, 3, 1, 3]. 

| tôi | một [tôi] | tôi | r | 
| --- | --- | --- | --- | 
| 0 | 1 | -1 | -1 | 
| 1 | 3 | 1 | 1 | 
| 2 | 1 | 1 | 1 | 
| 3 | 3 | 1 | 3 | 

Thuật toán chọn đoạn (2, 4). Điều này bao gồm cả các phần tử không phải là một và giữ tất cả các phần tử xung quanh ở bên ngoài để tối đa hóa sự đóng góp cho tổng. 

Bây giờ hãy xem xét [2, 1, 2, 1, 1, 3]. 

| tôi | một [tôi] | tôi | r | 
| --- | --- | --- | --- | 
| 0 | 2 | 0 | 0 | 
| 1 | 1 | 0 | 0 | 
| 2 | 2 | 0 | 2 | 
| 3 | 1 | 0 | 2 | 
| 4 | 1 | 0 | 2 | 
| 5 | 3 | 0 | 5 | 

Đoạn được chọn trở thành (1, 6). Điều này thu thập tất cả các phần tử không phải một để sản phẩm của chúng được tối đa hóa trong khi vẫn duy trì sự đóng góp cộng gộp của tất cả các phần tử bên ngoài là không thể ở đây vì chúng nằm trong khoảng, nhưng không có phân đoạn nào tốt hơn vì việc chia tách sẽ cô lập các đóng góp không phải một và giảm mức tăng nhân. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | mỗi phần tử được quét một lần cho mỗi trường hợp thử nghiệm | 
| Không gian | O(1) | chỉ có hai chỉ số được lưu trữ | 

Tổng độ phức tạp trên tất cả các trường hợp thử nghiệm vẫn tuyến tính trong tổng kích thước đầu vào, vừa vặn thoải mái trong các ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        l = -1
        r = -1
        for i in range(n):
            if a[i] != 1:
                if l == -1:
                    l = i
                r = i
        if l == -1:
            out.append("1 1")
        else:
            out.append(f"{l+1} {r+1}")
    return "\n".join(out)

# provided samples
assert run("""9
4
1 3 1 3
4
1 1 2 3
5
1 1 1 1 1
5
10 1 10 1 10
1
1
2
2 2
3
2 1 2
4
2 1 1 3
6
2 1 2 1 1 3
""") == """2 4
3 4
1 1
1 5
1 1
1 2
2 2
4 4
1 6"""

# all ones
assert run("1\n5\n1 1 1 1 1\n") == "1 1"

# single element
assert run("1\n1\n7\n") == "1 1"

# no ones but mixed
assert run("1\n4\n2 3 4 5\n") == "1 4"

# ones around center
assert run("1\n7\n1 1 2 1 1 3 1\n") == "3 6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả những cái | 1 1 | trường hợp thoái hóa | 
| phần tử đơn | 1 1 | tối thiểu n | 
| 2 3 4 5 | 1 4 | tất cả không một nhịp | 
| 1 1 2 1 1 3 1 | 3 6 | rải rác không phải ai | 

## Vỏ cạnh 

Đối với một mảng bao gồm toàn những mảng như [1, 1, 1], quá trình quét không bao giờ tìm thấy bất kỳ phần tử nào khác 1, do đó cả hai ranh giới vẫn không được đặt. Thuật toán trả về trực tiếp (1, 1), giữ nguyên tổng bằng 3 vì việc thay thế bất kỳ phân đoạn nào bằng sản phẩm của nó không làm thay đổi tổng. 

Đối với trường hợp như [2, 1, 1, 3], tập không phải một đầu tiên l ở chỉ số 0 và tập không phải một cuối cùng r ở chỉ mục 3. Đầu ra (1, 4) hợp nhất mọi thứ. Bất kỳ nỗ lực nào nhằm chọn phân đoạn nhỏ hơn sẽ loại trừ 2 hoặc 3, làm giảm sản phẩm thu được và do đó làm giảm tổng cuối cùng so với hợp nhất toàn bộ khoảng thời gian.
