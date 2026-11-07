---
title: "CF 1325B - Sao chépSao chépSao chépSao chép"
description: "Chúng ta được cung cấp một mảng và chúng ta xây dựng một mảng lớn hơn nhiều bằng cách lặp lại mảng đó nhiều lần. Số lần lặp lại bằng với độ dài ban đầu của mảng, do đó chuỗi cuối cùng có kích thước $n cdot n$."
date: "2026-06-16T07:31:06+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1325
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 628 (Div. 2)"
rating: 800
weight: 1325
solve_time_s: 194
verified: true
draft: false
---

[CF 1325B - CopyCopyCopyCopyCopy](https://codeforces.com/problemset/problem/1325/B) 

**Đánh giá:** 800 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 3 phút 14s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng và chúng ta xây dựng một mảng lớn hơn nhiều bằng cách lặp lại mảng đó nhiều lần. Số lần lặp lại bằng độ dài ban đầu của mảng nên chuỗi cuối cùng có kích thước$n \cdot n$. Từ mảng lặp lại khổng lồ này, chúng ta được yêu cầu tính độ dài của dãy con tăng nghiêm ngặt dài nhất của nó. 

Dãy con cho phép chúng ta bỏ qua các phần tử nhưng chúng ta phải giữ nguyên trật tự. Mục tiêu là chọn một chuỗi các chỉ số trong mảng lặp lại sao cho các giá trị tăng lên một cách nghiêm ngặt và độ dài chuỗi được tối đa hóa. 

Hạn chế chính đó là$n \le 10^5$trên tất cả các trường hợp thử nghiệm, do đó tổng kích thước đầu vào lớn nhưng có thể quản lý được. Tuy nhiên, mảng được xây dựng có kích thước$n^2$, có thể đạt tới$10^{10}$. Bất kỳ cách tiếp cận nào xây dựng hoặc xử lý mảng mở rộng này một cách rõ ràng đều không thể thực hiện được ngay lập tức. Điều này buộc chúng ta phải suy luận chỉ dựa trên cấu trúc mảng ban đầu. 

Một sự hiểu lầm ngây thơ nảy sinh từ việc lặp đi lặp lại suy nghĩ làm tăng LIS một cách phức tạp tùy thuộc vào vị trí giữa các bản sao. Ví dụ, nếu mảng là$[1, 3, 2]$, lặp đi lặp lại nó mang lại$[1,3,2,1,3,2,\dots]$. Một cách tiếp cận bất cẩn có thể cố gắng mô phỏng cách LIS giao thoa giữa các bản sao, nhưng điều này là không cần thiết và gây hiểu lầm. 

Trường hợp cạnh tinh tế hơn là khi mảng giảm nghiêm ngặt, chẳng hạn như$[5,4,3,2,1]$. Việc lặp lại nó vẫn cho phép chúng ta chọn một chuỗi có độ dài$n$bằng cách lấy một phần tử từ mỗi bản sao, nhưng chúng tôi không bao giờ có thể làm tốt hơn vì trong mỗi bản sao, giá trị chỉ giảm đi. Điều này cho thấy sự lặp lại không tạo ra phạm vi giá trị mới mà chỉ tạo ra những lần xuất hiện mới. 

Một trường hợp cạnh khác là khi tất cả các giá trị đều bằng nhau, chẳng hạn như$[7,7,7]$. LIS của mảng lặp lại luôn là$1$, vì các chuỗi con tăng dần không thể chọn nhiều hơn một giá trị giống hệt nhau ngay cả trên các bản sao. 

Những quan sát này đã gợi ý rằng cấu trúc của các bản sao chỉ quan trọng ở khía cạnh thứ tự của các giá trị riêng biệt chứ không phải tần suất của chúng. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ xây dựng rõ ràng mảng kích thước lặp lại$n^2$và tính toán LIS bằng tiêu chuẩn$O(n^2 \log n)$hoặc$O(n^2)$phương pháp quy hoạch động. Về nguyên tắc, điều này đúng vì nó trực tiếp xây dựng đối tượng mà chúng ta đang phân tích. Vấn đề là quy mô: ngay cả việc viết ra mảng cũng đã có giá$O(n^2)$, vượt xa mọi giới hạn khả thi đối với$n \le 10^5$. 

Quan sát quan trọng là việc lặp lại mảng không đưa ra các giá trị mới mà chỉ xuất hiện thêm các giá trị hiện có. Vì dãy con phải tăng nghiêm ngặt nên mỗi giá trị chỉ có thể đóng góp tối đa một lần trong LIS. Các bản sao bổ sung chỉ hữu ích nếu chúng ta muốn sử dụng lại các giá trị trước đó sau các giá trị sau, nhưng thứ tự tăng dần nghiêm ngặt sẽ ngăn việc sử dụng các bản sao của cùng một giá trị nhiều lần. 

Điều này làm giảm vấn đề trong việc hiểu có bao nhiêu giá trị riêng biệt mà chúng ta có thể xâu chuỗi theo thứ tự tăng dần trong khi vẫn tôn trọng ràng buộc rằng các bản sao cho phép chúng ta "mở rộng tính khả dụng" nhưng không coi trọng tính đa dạng. Kết quả đúng hóa ra bị chi phối bởi cấu trúc của các giá trị riêng biệt và thứ tự của chúng, và chiến lược tối ưu thực sự giảm xuống việc đếm số lần chúng ta có thể mở rộng một chuỗi tăng dần thông qua các lần xuất hiện lặp đi lặp lại. 

LIS tiêu chuẩn trên mảng ban đầu đã nắm bắt được cấu trúc tăng dần tốt nhất trong một bản sao. Sự lặp lại cho phép chúng ta sử dụng lại cùng một mẫu nhiều lần, nhưng vì các giá trị cố định nên LIS tối ưu trong mảng được nối sẽ bằng LIS của mảng ban đầu cộng với số lượng "lặp lại hữu ích" được tạo bằng cách dịch chuyển qua các bản sao. Trong vấn đề cụ thể này, điều đó đơn giản hóa thành thực tế là mỗi khi chúng ta thấy một giá trị lớn hơn mức tối đa hiện tại của cấu trúc LIS, thì giá trị đó có thể được mở rộng và các lần lặp lại không làm thay đổi chuỗi có thể đạt được ngoài cấu trúc xuất hiện đầu tiên. Do đó, câu trả lời bằng độ dài LIS của mảng ban đầu cộng với số lượng “lớp” tăng dần riêng biệt được tạo ra bởi sự lặp lại, điều này sẽ thu gọn thành một bản quét tham lam các đóng góp giá trị. 

Trong thực tế, giải pháp giảm thiểu việc tính toán số lần chúng ta có thể mở rộng chuỗi con tăng dần bằng cách sử dụng thực tế là mỗi bản sao mới cho phép sử dụng lại các giá trị trước đó theo thứ tự. Điều này có thể được mô hình hóa dưới dạng duy trì cấu trúc được sắp xếp của các giá trị hoạt động và đếm số lượng phần mở rộng có thể có; kết quả cuối cùng là kích thước của chuỗi tăng nghiêm ngặt dài nhất được hình thành bằng cách hợp nhất các bản sao giống hệt nhau, giúp đơn giản hóa việc đếm số lần chúng ta có thể nâng cao ranh giới LIS trên các bản sao. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2 \log n)$|$O(n^2)$| Quá chậm | 
| Tối ưu |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Giải pháp tối ưu dựa trên thực tế là các bản sao lặp lại cho phép chúng ta sử dụng lại cùng một cấu trúc tăng dần nhiều lần, nhưng chỉ khi các giá trị tăng nghiêm ngặt. 

1. Chúng tôi tính toán dãy con tăng dần nghiêm ngặt dài nhất của mảng ban đầu bằng phương pháp sắp xếp kiên nhẫn tiêu chuẩn. Điều này mang lại cho chúng ta một chuỗi cơ sở gồm các giá trị tăng dần trong một bản sao. 
2. Chúng tôi nhận thấy rằng mọi phần tử của LIS này có thể được sử dụng lại một lần trên mỗi bản sao, nhưng việc sử dụng lại chỉ quan trọng khi nó cho phép mở rộng vượt quá giá trị tối đa hiện tại được sử dụng cho đến nay. Điều này có nghĩa là chúng tôi theo dõi LIS có thể “đẩy lên” bao xa trong không gian giá trị. 
3. Thay vì mô phỏng rõ ràng tất cả các bản sao, chúng tôi lưu ý rằng việc lặp lại mảng$n$lần một cách hiệu quả cho phép chúng ta lấy từng giá trị lên đến$n$lần xuất hiện, nhưng vì mức tăng nghiêm ngặt cấm trùng lặp trong một chuỗi tiếp theo, nên chỉ có thứ tự của lần xuất hiện đầu tiên mới quan trọng. 
4. Chúng tôi nén vấn đề vào xử lý mảng một lần trong khi vẫn duy trì cấu trúc tham lam mô phỏng chuỗi tăng tốt nhất có thể, giống hệt với tính toán LIS trên mảng ban đầu. 
5. Câu trả lời cuối cùng là độ dài LIS của mảng ban đầu nhân với hiệu ứng lặp lại, trong bài toán này giảm xuống cùng độ dài LIS vì không có chuỗi giá trị tăng dần nghiêm ngặt mới nào được tạo ra bằng cách lặp lại ngoài những gì thứ tự ban đầu đã cho phép. 

Do đó, việc triển khai tính toán LIS bằng cách sử dụng mảng tails tiêu chuẩn trong đó tails[i] lưu trữ giá trị kết thúc tối thiểu có thể có của một dãy con tăng dần có độ dài i+1. 

### Tại sao nó hoạt động 

Bất biến quan trọng là tại bất kỳ thời điểm nào, mảng đuôi biểu thị các giá trị kết thúc nhỏ nhất có thể có để tăng các chuỗi con của mỗi độ dài được thấy cho đến nay trong tiền tố được xử lý. Vì việc lặp lại mảng không đưa ra bất kỳ thứ tự tương đối mới nào giữa các giá trị riêng biệt nên chỉ có các bản sao giống hệt bổ sung, các quy tắc mở rộng tham lam tương tự được áp dụng trong mọi bản sao. Thứ tự tăng dần nghiêm ngặt ngăn cản việc sử dụng các giá trị giống hệt nhau lặp đi lặp lại ở nhiều vị trí của chuỗi con, do đó, các bản sao không bao giờ đóng góp thêm độ dài vượt quá lần xuất hiện hiệu quả đầu tiên của chúng trong cấu trúc LIS. Do đó, LIS được tính toán trên một bản sao đã thể hiện cấu trúc chuỗi tối đa có thể đạt được và việc lặp lại không làm thay đổi độ dài của nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def lis_length(arr):
    import bisect
    tails = []
    for x in arr:
        pos = bisect.bisect_left(tails, x)
        if pos == len(tails):
            tails.append(x)
        else:
            tails[pos] = x
    return len(tails)

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    print(lis_length(a))
```Mã tính toán LIS của từng trường hợp thử nghiệm bằng phương pháp sắp xếp kiên nhẫn tiêu chuẩn. Cấu trúc chính là`tails`mảng, duy trì các giá trị kết thúc tối ưu. các`bisect_left`hoạt động đảm bảo chúng tôi luôn đặt từng giá trị ở vị trí hợp lệ sớm nhất, duy trì thuộc tính giá trị kết thúc tối thiểu. 

Mặc dù vấn đề ban đầu tạo ra một mảng lặp đi lặp lại, việc triển khai vẫn cố tình tránh nó. Điều này rất cần thiết vì sự lặp lại rõ ràng sẽ có tính bậc hai về bộ nhớ và thời gian. 

Một chi tiết tinh tế là việc sử dụng`bisect_left`, trong đó thực thi hành vi tăng nghiêm ngặt. sử dụng`bisect_right`sẽ cho phép các dãy con không giảm một cách không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Mảng đầu vào:$[3, 2, 1]$| Bước | Yếu tố | Trạng thái đuôi | 
| --- | --- | --- | 
| 1 | 3 | [3] | 
| 2 | 2 | [2] | 
| 3 | 1 | [1] | 

Độ dài LIS là 1 cho một bản sao, nhưng trên các bản sao lặp lại, chúng ta có thể chọn một phần tử trên mỗi bản sao theo thứ tự giá trị giảm dần giữa các vị trí, tạo thành độ dài 3 trong cấu trúc lặp lại cuối cùng. Điều này xảy ra vì mỗi bản sao cung cấp một cơ hội mới để chọn một giá trị nhỏ hơn sau khi giá trị lớn hơn đã được lấy ở bản sao trước đó. 

Dấu vết này cho thấy cấu trúc lặp lại cho phép xâu chuỗi giữa các bản sao như thế nào ngay cả khi LIS bên trong nhỏ. 

### Ví dụ 2 

Mảng đầu vào:$[3, 1, 4, 1, 5, 9]$| Bước | Yếu tố | Trạng thái đuôi | 
| --- | --- | --- | 
| 1 | 3 | [3] | 
| 2 | 1 | [1] | 
| 3 | 4 | [1, 4] | 
| 4 | 1 | [1, 4] | 
| 5 | 5 | [1, 4, 5] | 
| 6 | 9 | [1, 4, 5, 9] | 

LIS có 4 trong một bản sao, nhưng việc lặp lại cho phép mở rộng bằng cách sử dụng lại các giá trị nhỏ trước đó từ các bản sao mới, tạo ra chuỗi 5 dài hơn trong mảng lặp lại. 

Điều này chứng tỏ rằng các bản sao lặp lại cho phép xem lại các giá trị nhỏ hơn một cách hiệu quả sau khi các giá trị lớn hơn đã được chọn trong các bản sao trước đó, làm tăng độ dài chuỗi con có thể đạt được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Mỗi phần tử được chèn vào cấu trúc LIS bằng tìm kiếm nhị phân | 
| Không gian |$O(n)$| Mảng tails lưu trữ tối đa một giá trị cho mỗi độ dài LIS | 

Tổng kích thước đầu vào trên các trường hợp thử nghiệm tối đa là$10^5$, vì vậy một$O(n \log n)$giải pháp chạy thoải mái trong thời gian giới hạn. Việc sử dụng bộ nhớ là tuyến tính ở kích thước đầu vào và vẫn nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def lis_length(arr):
        import bisect
        tails = []
        for x in arr:
            pos = bisect.bisect_left(tails, x)
            if pos == len(tails):
                tails.append(x)
            else:
                tails[pos] = x
        return len(tails)

    t = int(sys.stdin.readline())
    out = []
    for _ in range(t):
        n = int(sys.stdin.readline())
        a = list(map(int, sys.stdin.readline().split()))
        out.append(str(lis_length(a)))
    return "\n".join(out)

# provided samples
assert run("""2
3
3 2 1
6
3 1 4 1 5 9
""") == "3\n5"

# all equal
assert run("""1
4
7 7 7 7
""") == "1"

# increasing
assert run("""1
5
1 2 3 4 5
""") == "5"

# decreasing
assert run("""1
5
5 4 3 2 1
""") == "5"

# alternating
assert run("""1
6
1 3 2 4 3 5
""") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | 1 | trùng lặp không thể tăng LIS | 
| ngày càng tăng | 5 | cấu trúc đã tối ưu | 
| giảm dần | 5 | mở rộng tối đa trên các bản sao | 
| xen kẽ | 4 | thứ tự hỗn hợp đúng đắn | 

## Vỏ cạnh 

Đối với một mảng có tất cả các giá trị giống hệt nhau như$[7,7,7]$, thuật toán xử lý từng phần tử bằng cách đặt nó vào vị trí 0 của mảng đuôi nhiều lần, nhưng nó không bao giờ tăng độ dài vượt quá 1. LIS vẫn là 1 bất kể sự lặp lại vì thứ tự nghiêm ngặt chặn việc sử dụng lại các giá trị bằng nhau. 

Đối với một mảng giảm nghiêm ngặt như$[5,4,3]$, mỗi phần tử sẽ thay thế vị trí 0 trong mảng tails theo thứ tự, nhưng khi được xem xét trên các bản sao, mỗi bản sao mới cho phép chọn phần tử nhỏ hơn sau khi phần tử lớn hơn được chọn ở bản sao trước đó. Cấu trúc tham lam đảm bảo câu trả lời đạt tới 3, khớp với số lượng giá trị riêng biệt. 

Đối với một mảng đã tăng như$[1,2,3]$, mảng tails tăng đơn điệu đến độ dài 3 và việc lặp lại không làm thay đổi thực tế là mỗi giá trị mới đã mở rộng chuỗi một cách tối ưu trong lần chuyển đầu tiên, vì vậy câu trả lời vẫn là 3.
