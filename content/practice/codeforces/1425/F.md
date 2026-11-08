---
title: "CF 1425F - Chim Hồng Hạc Bí Ẩn"
description: "Chúng ta có một mảng không xác định $A1, A2, dots, AN$, trong đó $Ai$ là số lượng chim hồng hạc trong lồng $i$. Hoạt động duy nhất có sẵn là yêu cầu tổng của một phân đoạn liền kề. Truy vấn có dạng $(L,R)$ trả về $$AL + A{L+1} + cdots + AR$$ với hạn chế là $L < R$."
date: "2026-06-11T05:54:55+07:00"
tags: ["codeforces", "competitive-programming", "interactive"]
categories: ["algorithms"]
codeforces_contest: 1425
codeforces_index: "F"
codeforces_contest_name: "2020 ICPC, COMPFEST 12, Indonesia Multi-Provincial Contest (Unrated, Online Mirror, ICPC Rules, Teams Preferred)"
rating: 1400
weight: 1425
solve_time_s: 138
verified: false
draft: false
---

[CF 1425F - Chim hồng hạc bí ẩn](https://codeforces.com/problemset/problem/1425/F) 

**Đánh giá:** 1400 
**Thẻ:** tương tác 
**Thời gian giải:** 2m 18s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một mảng không xác định$A_1, A_2, \dots, A_N$, Ở đâu$A_i$là số lượng chim hồng hạc trong lồng$i$. 

Hoạt động duy nhất có sẵn là yêu cầu tổng của một phân đoạn liền kề. Một truy vấn có dạng$(L,R)$trả lại$$A_L + A_{L+1} + \cdots + A_R$$với hạn chế đó$L < R$. 

Chúng tôi được phép nhiều nhất$N$truy vấn và phải xây dựng lại chính xác từng phần tử của mảng. 

Mặc dù nhiệm vụ ban đầu có tính tương tác nhưng một khi chúng ta hiểu được chiến lược thì việc thực hiện sẽ rất đơn giản. Thử thách là thiết kế một bộ tối đa$N$truy vấn tổng phạm vi xác định duy nhất tất cả các giá trị. 

Các ràng buộc đủ nhỏ để việc lưu trữ toàn bộ mảng là chuyện nhỏ. Hạn chế thực sự là ngân sách truy vấn. Vì chỉ$N$được phép đặt câu hỏi, bất kỳ chiến lược nào yêu cầu$O(N^2)$hoặc thậm chí$2N$truy vấn là không thể. 

Quan sát quan trọng là mảng chứa$N$giá trị chưa biết, vì vậy chúng tôi cần khoảng$N$các phương trình độc lập. Bài toán trở thành một hệ phương trình tuyến tính nhỏ được xây dựng từ các tổng phạm vi. 

Một lỗi phổ biến là cố gắng khôi phục từng phần tử bằng các tổng liền kề. Ví dụ, truy vấn$$(1,2), (2,3), (3,4), \dots$$chỉ cho$N-1$phương trình. Điều đó không đủ để xác định duy nhất$N$những điều chưa biết. 

Coi như$N=3$. 

Giả sử chúng ta biết$$A_1+A_2=5$$Và$$A_2+A_3=7.$$Cả hai mảng$[0,5,2]$Và$[1,4,3]$thỏa mãn các phương trình này. Giải pháp không phải là duy nhất. 

Một trường hợp tinh tế khác là khi một số giá trị bằng 0. 

Ví dụ,$$[0,0,5].$$Phương pháp tái thiết không được dựa vào giá trị dương hoặc phép chia. Chúng ta cần các đồng nhất thức đại số thuần túy liên quan đến tổng và phép trừ. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là truy vấn từng cặp hoặc nhiều phạm vi chồng chéo cho đến khi có đủ thông tin. Điều này chắc chắn có tác dụng vì tổng phạm vi xác định mảng nhưng nó nhanh chóng vượt quá giới hạn truy vấn. Hỏi tất cả các số tiền liền kề đã có chi phí$N-1$truy vấn và vẫn không xác định được mảng duy nhất. Yêu cầu phạm vi bổ sung cho mọi vị trí sẽ yêu cầu$O(N)$truy vấn thêm, vượt quá ngân sách. 

Cấu trúc của bài toán đề xuất coi mỗi câu trả lời truy vấn là một phương trình tuyến tính. 

Ba yếu tố đầu tiên là đặc biệt. Nếu chúng ta biết$$S_{12}=A_1+A_2,$$

$$S_{13}=A_1+A_2+A_3,$$

$$S_{23}=A_2+A_3,$$thì ta có ba phương trình với ba ẩn số. 

Cộng phương trình thứ nhất và thứ ba sẽ được$$S_{12}+S_{23}=A_1+2A_2+A_3.$$Trừ$S_{13}$lá chính xác$A_2$:$$A_2=S_{12}+S_{23}-S_{13}.$$Một lần$A_2$được biết,$$A_1=S_{12}-A_2,$$Và$$A_3=S_{23}-A_2.$$Vì vậy, chỉ có ba truy vấn được lựa chọn cẩn thận mới xác định được hoàn toàn ba lồng đầu tiên. 

Sau đó, việc khôi phục các giá trị còn lại trở nên dễ dàng. Truy vấn$$(1,i)$$cho mọi$i \ge 4$. 

Gọi số tiền trả về là$$P_i=A_1+A_2+\cdots+A_i.$$Vì chúng ta đã biết tổng tiền tố lên đến$i-1$,$$A_i=P_i-P_{i-1}.$$Chúng tôi sử dụng chính xác ba truy vấn cho ba phần tử đầu tiên và một truy vấn cho mỗi phần tử còn lại:$$3+(N-3)=N.$$Điều này phù hợp chính xác với ngân sách cho phép. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Truy vấn O(N²) trở lên | O(N) | Không thể | 
| Tối ưu | Truy vấn O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Truy vấn tổng các phạm vi$(1,2)$,$(1,3)$, Và$(2,3)$. Gọi câu trả lời$s_{12}$,$s_{13}$, Và$s_{23}$. 
2. Khôi phục giá trị thứ hai bằng cách sử dụng$$A_2=s_{12}+s_{23}-s_{13}.$$Điều này hoạt động vì$A_1$Và$A_3$Hủy bỏ. 

1. Phục hồi$$A_1=s_{12}-A_2$$Và$$A_3=s_{23}-A_2.$$Bây giờ ba phần tử đầu tiên đã được biết chính xác. 

1. Tính toán$$pref=A_1+A_2+A_3.$$Điều này bằng tổng tiền tố đến vị trí thứ ba. 

1. Đối với mọi vị trí$i$từ$4$ĐẾN$N$, truy vấn phạm vi$(1,i)$. 
2. Hãy để câu trả lời là$cur$. Từ$cur$bằng tổng tiền tố thông qua$i$,$$A_i=cur-pref.$$1. Cập nhật$$pref=cur.$$Bây giờ tổng tiền tố thông qua$i$đã được biết và có thể được sử dụng cho vị trí tiếp theo. 

1. Sau khi tất cả các giá trị được xây dựng lại, hãy xuất toàn bộ mảng. 

### Tại sao nó hoạt động 

Ba truy vấn đầu tiên tạo thành một hệ thống tuyến tính hoàn chỉnh cho$A_1$,$A_2$, Và$A_3$. Các công thức dẫn xuất phục hồi các giá trị này một cách duy nhất. 

Đối với mọi vị trí sau này$i$, truy vấn$(1,i)$trả về tổng tiền tố thông qua$i$. Thuật toán duy trì tính bất biến`pref`bằng tổng tiền tố thông qua$i-1$. Trừ hai tổng tiền tố liên tiếp này sẽ cho kết quả chính xác$A_i$. Vì bất biến ban đầu đúng sau khi xây dựng lại ba phần tử đầu tiên nên nó vẫn đúng cho mọi bước. Mỗi phần tử mảng được khôi phục chính xác một lần, vì vậy câu trả lời cuối cùng là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def ask(l, r):
    print("?", l, r, flush=True)
    return int(input())

def main():
    n = int(input())

    s12 = ask(1, 2)
    s13 = ask(1, 3)
    s23 = ask(2, 3)

    a = [0] * n

    a[1] = s12 + s23 - s13
    a[0] = s12 - a[1]
    a[2] = s23 - a[1]

    pref = a[0] + a[1] + a[2]

    for i in range(4, n + 1):
        cur = ask(1, i)
        a[i - 1] = cur - pref
        pref = cur

    print("!", *a, flush=True)

if __name__ == "__main__":
    main()
```Phần đầu tiên của mã thu được ba phương trình cần thiết để giải ba giá trị đầu tiên. Công thức cho`a[1]`trực tiếp đến từ việc loại bỏ$A_1$Và$A_3$. 

Sau đó,`pref`lưu trữ tổng tiền tố đã được xây dựng lại. Mỗi truy vấn`(1, i)`trả về tổng tiền tố lớn hơn. Sự khác biệt giữa các tổng tiền tố liên tiếp chính xác là phần tử mới. 

Việc lập chỉ mục xứng đáng được chú ý. Mảng được lưu trữ với lập chỉ mục dựa trên 0, trong khi các truy vấn sử dụng lập chỉ mục dựa trên một. Chức vụ`i`trong bài toán tương ứng với`a[i - 1]`trong mã. 

Một chi tiết khác là xả nước. Thẩm phán tương tác không xử lý truy vấn cho đến khi nó được gửi đi. sử dụng`flush=True`đảm bảo câu hỏi sẽ đến tay thẩm phán ngay lập tức. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Giả sử mảng ẩn là:$$[1,4,4,6,7,8]$$| Bước | Truy vấn | Trả lời | Giá trị được phục hồi | trước | 
| --- | --- | --- | --- | --- | 
| 1 | (1,2) | 5 | - | - | 
| 2 | (1,3) | 9 | - | - | 
| 3 | (2,3) | 8 | A₂=4, A₁=1, A₃=4 | 9 | 
| 4 | (1,4) | 15 | A₄=15−9=6 | 15 | 
| 5 | (1,5) | 22 | A₅=22−15=7 | 22 | 
| 6 | (1,6) | 30 | A₆=30−22=8 | 30 | 

Mảng cuối cùng:$$[1,4,4,6,7,8].$$Dấu vết này cho thấy cách mỗi phần tử sau này thu được từ chênh lệch của các tổng tiền tố liên tiếp. 

### Ví dụ 2 

Giả sử mảng ẩn là:$$[0,0,5].$$| Bước | Truy vấn | Trả lời | 
| --- | --- | --- | 
| (1,2) | 0 | | 
| (1,3) | 5 | | 
| (2,3) | 5 | | 

Sự hồi phục:$$A_2=0+5-5=0$$

$$A_1=0-0=0$$

$$A_3=5-0=5$$Mảng cuối cùng:$$[0,0,5].$$Ví dụ này chứng minh rằng phương pháp này hoạt động ngay cả khi một số lồng không chứa chim hồng hạc nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Một bước tái thiết cho mỗi phần tử | 
| Không gian | O(N) | Lưu trữ mảng đã khôi phục | 
| Truy vấn | O(N) | Chính xác N truy vấn được sử dụng | 

Thuật toán hỏi chính xác$N$câu hỏi phù hợp với giới hạn cho phép. Việc sử dụng bộ nhớ là tuyến tính theo số lượng lồng, dễ dàng phù hợp với các giới hạn. 

## Trường hợp thử nghiệm 

Vấn đề ban đầu là do tính tương tác nên không thể áp dụng khai thác thử nghiệm ngoại tuyến thông thường. Một cách hữu ích để kiểm tra các công thức tái thiết là mô phỏng người đánh giá.```
def reconstruct(arr):
    n = len(arr)

    s12 = arr[0] + arr[1]
    s13 = arr[0] + arr[1] + arr[2]
    s23 = arr[1] + arr[2]

    ans = [0] * n

    ans[1] = s12 + s23 - s13
    ans[0] = s12 - ans[1]
    ans[2] = s23 - ans[1]

    pref = sum(ans[:3])

    for i in range(3, n):
        cur = sum(arr[:i + 1])
        ans[i] = cur - pref
        pref = cur

    return ans

# minimum size
assert reconstruct([0, 0, 0]) == [0, 0, 0]

# sample-style case
assert reconstruct([1, 4, 4, 6, 7, 8]) == [1, 4, 4, 6, 7, 8]

# all equal
assert reconstruct([5, 5, 5, 5, 5]) == [5, 5, 5, 5, 5]

# zeros mixed with positives
assert reconstruct([0, 7, 0, 3, 0]) == [0, 7, 0, 3, 0]

# larger values near limit
assert reconstruct([1000] * 10) == [1000] * 10
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| [0,0,0] | [0,0,0] | Kích thước tối thiểu và tất cả số không | 
| [1,4,4,6,7,8] | Cùng một mảng | Tái thiết điển hình | 
| [5,5,5,5,5] | Cùng một mảng | Giá trị lặp lại | 
| [0,7,0,3,0] | Cùng một mảng | Giá trị 0 bên trong mảng | 
| [1000,…,1000] | Cùng một mảng | Ranh giới giá trị trên | 

## Vỏ cạnh 

### Mảng kích thước tối thiểu 

lấy$$[2,5,7].$$Ba truy vấn ban đầu đã xác định tất cả các giá trị:$$s_{12}=7,\quad s_{13}=14,\quad s_{23}=12.$$Sau đó$$A_2=7+12-14=5,$$

$$A_1=7-5=2,$$

$$A_3=12-5=7.$$Không có truy vấn bổ sung là cần thiết. 

### Mảng chứa số 0 

lấy$$[0,0,5,0].$$Ba phương trình đầu tiên trở thành$$s_{12}=0,\quad s_{13}=5,\quad s_{23}=5.$$Phục hồi mang lại$$A_2=0,\quad A_1=0,\quad A_3=5.$$Truy vấn$(1,4)$trả lại$5$. Vì tổng tiền tố hiện tại đã$5$,$$A_4=5-5=0.$$Phương pháp này không bao giờ giả định tính tích cực. 

### Giá trị lớn bằng nhau 

lấy$$[1000,1000,1000,1000].$$Các truy vấn đầu tiên trả về$$2000,\ 3000,\ 2000.$$Phục hồi mang lại$$A_2=1000,\quad A_1=1000,\quad A_3=1000.$$Truy vấn tiền tố$(1,4)$trả lại$4000$, mang lại$$A_4=4000-3000=1000.$$Chỉ sử dụng phép cộng và phép trừ nên không có vấn đề về số.
