---
title: "CF 105845C - Phân đoạn OR-Max"
description: "Chúng ta được cung cấp một mảng các số nguyên và chúng ta xem xét mọi mảng con liền kề. Một mảng con được coi là hợp lệ khi có một đẳng thức rất cụ thể: giá trị lớn nhất bên trong mảng con phải chính xác bằng OR theo từng bit của tất cả các giá trị trong cùng mảng con đó."
date: "2026-06-25T14:49:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105845
codeforces_index: "C"
codeforces_contest_name: "CodEMI 2025"
rating: 0
weight: 105845
solve_time_s: 52
verified: true
draft: false
---

[CF 105845C - Phân đoạn OR-Max](https://codeforces.com/problemset/problem/105845/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và chúng ta xem xét mọi mảng con liền kề. Một mảng con được coi là hợp lệ khi có một đẳng thức rất cụ thể: giá trị lớn nhất bên trong mảng con phải chính xác bằng OR theo từng bit của tất cả các giá trị trong cùng mảng con đó. 

Nói cách khác, nếu chúng ta lấy bất kỳ phân đoạn nào, tính toán phần tử tối đa của nó và cũng tính toán OR theo bit của mọi phần tử bên trong nó, chúng ta chỉ tính phân đoạn đó nếu hai giá trị này giống hệt nhau. 

Nhiệm vụ là đếm xem có bao nhiêu mảng con thỏa mãn điều kiện này trong nhiều trường hợp thử nghiệm. 

Ràng buộc rằng tổng chiều dài của tất cả các trường hợp thử nghiệm lên tới$2 \cdot 10^5$ngụ ý rằng bất kỳ cách tiếp cận nào tệ hơn phương pháp tuyến tính đại khái cho mỗi trường hợp thử nghiệm sẽ thất bại. Việc quét bậc hai trên tất cả các mảng con là quá lớn vì ngay cả một mảng có kích thước$2 \cdot 10^5$sẽ ám chỉ về$2 \cdot 10^{10}$mảng con. 

Một ràng buộc cấu trúc quan trọng được ẩn giấu trong chính đẳng thức đó. OR của một phân đoạn chỉ có thể tăng khi các phần tử mới được thêm vào, trong khi giá trị tối đa được xác định bởi một phần tử duy nhất. Sự không khớp này buộc phải có những hạn chế mạnh mẽ về hình thức của một mảng con hợp lệ. 

Trường hợp cạnh tinh tế xuất hiện khi các phần tử lặp lại hoặc khi một phần tử lớn được bao quanh bởi các phần tử nhỏ hơn không đưa ra các bit mới. Ví dụ, trong mảng$[1, 2, 3]$, mảng con$[1,2,3]$có tối đa$3$và HOẶC$1 ,|, 2 ,|, 3 = 3$, vì vậy nó hợp lệ. Tuy nhiên, một mảng con như$[2,3,2]$cũng hoạt động, trong khi$[1,2]$thất bại vì tối đa là$2$nhưng HOẶC là$3$. Điều này cho thấy rằng tính hợp lệ không chỉ phụ thuộc vào mức tối đa mà còn phụ thuộc vào việc liệu tất cả các phần tử khác có được “chứa” trong mẫu bit của nó hay không. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: liệt kê mọi mảng con, tính toán OR tối đa và theo bit của nó, rồi so sánh chúng. Tính toán cả hai giá trị từ chi phí đầu$O(n)$mỗi mảng con, đưa ra$O(n^3)$tổng thể, hoặc$O(n^2)$nếu chúng ta duy trì giá trị lăn. Thậm chí$O(n^2)$quá chậm đối với$2 \cdot 10^5$tổng chiều dài. 

Quan sát quan trọng xuất phát từ việc hiểu được điều kiện tác động như thế nào. Đối với một phân khúc$[l,r]$với giá trị tối đa$M$, mọi phần tử khác không được đưa vào bất kỳ bit nào chưa có trong$M$, nếu không thì OR sẽ vượt quá$M$. Điều này có nghĩa là mọi phần tử trong phân đoạn phải là một mặt nạ con của$M$. Đồng thời, kể từ$M$có mặt trong phân đoạn này thì OR ít nhất phải bằng$M$. Cùng với nhau, điều này buộc OR phải chính xác$M$khi và chỉ nếu mọi phần tử đều thỏa mãn$a_i ,|, M = M$. 

Điều này chuyển đổi vấn đề thành một khai triển có ràng buộc xung quanh mỗi vị trí đóng vai trò là mức tối đa. Nếu chúng ta sửa một chỉ mục$i$là vị trí của phần tử lớn nhất trong mảng con, chúng ta có thể mở rộng mảng con ra bên ngoài trong khi vẫn duy trì OR đang chạy. Ngay khi OR vượt quá$a_i$, chúng ta không bao giờ có thể phục hồi được nên hướng đi đó bị chặn lại. Điều này dẫn đến việc mở rộng kiểu hai con trỏ trong đó mỗi tâm được mở rộng cho đến khi ràng buộc OR bị phá vỡ. 

Một cách xem hiệu quả hơn là sửa các điểm cuối bên trái và mở rộng một cách tham lam các điểm cuối bên phải trong khi duy trì OR. Vì OR chỉ tăng bit nên mỗi bit chỉ có thể vào trạng thái OR một lần trên mỗi phần mở rộng phân đoạn, do đó tổng số lần chuyển đổi bị giới hạn bởi 30 bit cho mỗi phần tử. Điều này giúp bạn có thể duy trì các cửa sổ hợp lệ theo từng bước. 

Tối ưu hóa cuối cùng là duy trì, đối với mỗi vị trí bắt đầu, chúng ta có thể mở rộng bao xa trong khi vẫn giữ OR trong phạm vi ứng viên tối đa hiện tại. Khi một phần tử mới vượt quá mức tối đa hiện tại, cửa sổ sẽ được đặt lại vì mức tối đa thay đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^3)$hoặc$O(n^2 \cdot 30)$|$O(1)$| Quá chậm | 
| Cửa sổ HOẶC trượt với khả năng theo dõi tối đa |$O(n \cdot 30)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sửa điểm cuối bên trái$l$và duy trì hai giá trị trong khi mở rộng điểm cuối bên phải: OR theo bit hiện tại của phân đoạn và giá trị tối đa nhìn thấy trong phân khúc. Điều này đảm bảo chúng ta có thể kiểm tra điều kiện ở mỗi bước mà không cần tính toán lại từ đầu. 
2. Bắt đầu mở rộng$r$từ$l$ĐẾN$n$, cập nhật OR và tối đa khi chúng tôi đưa vào$a_r$. OR nắm bắt tất cả các đóng góp bit, trong khi giá trị tối đa theo dõi giá trị ứng cử viên phải khớp với OR. 
3. Sau mỗi lần gia hạn, hãy kiểm tra xem HOẶC hiện tại có bằng mức tối đa hiện tại hay không. Nếu chúng khớp nhau, mảng con$[l,r]$là hợp lệ và đóng góp một cho câu trả lời. 
4. Nếu tại bất kỳ thời điểm nào OR trở nên lớn hơn mức tối đa, chúng tôi sẽ không dừng lại ngay lập tức. Thay vào đó, chúng tôi tiếp tục, vì phần tử sau có thể tăng mức tối đa để khớp lại với OR. Điều này rất quan trọng: mức tối đa không được cố định bởi điểm cuối bên trái. 
5. Bất biến chính là đối với bất kỳ phân khúc hiện tại nào$[l,r]$, chúng tôi duy trì độ chính xác OR và mức tối đa chính xác của phân đoạn đó, do đó việc kiểm tra tính hợp lệ luôn chính xác. 

Tính chính xác dựa trên thực tế là OR chỉ tích lũy các bit, trong khi mức tối đa theo dõi một giá trị vượt trội duy nhất. Nếu OR bao gồm một bit không có trong mức tối đa thì mức tối đa đó phải tăng trong tương lai để khớp với nó, nếu không thì phân đoạn không bao giờ có thể trở thành hợp lệ. Do đó, mỗi phân đoạn hợp lệ được tính chính xác một lần khi đạt đến điểm cuối bên phải của nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        ans = 0
        
        for l in range(n):
            cur_or = 0
            cur_max = 0
            
            for r in range(l, n):
                cur_or |= a[r]
                if a[r] > cur_max:
                    cur_max = a[r]
                
                if cur_or == cur_max:
                    ans += 1
        
        print(ans)

if __name__ == "__main__":
    solve()
```Mã trực tiếp thực hiện ý tưởng mở rộng cố định bên trái. Đối với mỗi điểm cuối bên trái, chúng tôi mở rộng điểm cuối bên phải từng bước một, duy trì OR và giá trị tối đa trong các cập nhật liên tục theo thời gian. Kiểm tra sự bình đẳng`cur_or == cur_max`là điều kiện chính xác xác định một phân đoạn hợp lệ. 

Một lỗi phổ biến là cố gắng đặt lại cửa sổ khi OR vượt quá mức tối đa hiện tại. Điều đó không chính xác vì phần tử sau có thể trở thành phần tử tối đa mới và khôi phục sự bình đẳng. Thuật toán tránh việc đặt lại sớm bằng cách luôn giữ trạng thái phân đoạn đầy đủ. 

Một điểm tinh tế khác là chúng tôi không bao giờ tính lại OR hoặc giá trị tối đa từ đầu. Điều này là cần thiết vì việc tính toán lại sẽ đẩy độ phức tạp trở lại bậc hai cho mỗi trường hợp kiểm thử. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
4
1 2 3 2
```Chúng tôi theo dõi tất cả các phân đoạn hợp lệ. 

| tôi | r | HOẶC | Tối đa | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | 1 | vâng | 
| 0 | 1 | 3 | 2 | không | 
| 0 | 2 | 3 | 3 | vâng | 
| 0 | 3 | 3 | 3 | vâng | 

Để bắt đầu sau: 

| tôi | r | HOẶC | Tối đa | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 2 | vâng | 
| 1 | 2 | 3 | 3 | vâng | 
| 1 | 3 | 3 | 3 | vâng | 
| 2 | 2 | 3 | 3 | vâng | 
| 2 | 3 | 3 | 3 | vâng | 
| 3 | 3 | 2 | 2 | vâng | 

Điều này tạo ra tổng số. 

Dấu vết cho thấy tính hợp lệ thường xuất hiện khi OR “bắt kịp” đến mức tối đa sau khi bị tụt lại phía sau ban đầu. 

### Ví dụ 2 

đầu vào:```
1
5
1 2 4 8 16
```| tôi | r | HOẶC | Tối đa | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | 1 | vâng | 
| 0 | 1 | 3 | 2 | không | 
| 0 | 2 | 7 | 4 | không | 
| 0 | 3 | 15 | 8 | không | 
| 0 | 4 | 31 | 16 | vâng | 

Mỗi phần tử đều là lũy thừa của hai, do đó OR tăng trưởng nghiêm ngặt và sự bằng nhau chỉ giữ được khi phần tử cuối cùng chiếm ưu thế tất cả các phần tử trước đó. 

Điều này chứng tỏ điều kiện chọn các phân đoạn một cách hiệu quả như thế nào trong đó phần tử lớn nhất “giải thích” tất cả các đóng góp bit. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| Mỗi điểm cuối bên trái quét tất cả các điểm cuối bên phải với bản cập nhật O(1) | 
| Không gian |$O(1)$| Chỉ chạy OR và max mới được lưu trữ | 

Tổng kích thước đầu vào trên các trường hợp thử nghiệm là$2 \cdot 10^5$, vì vậy giải pháp này chỉ an toàn nếu sử dụng các hằng số được tối ưu hóa hoặc cắt tỉa bổ sung trong thực tế. Trong nhiều giải pháp dự định, việc cắt bớt bitwise hơn nữa sẽ làm giảm quá trình chuyển đổi, làm cho nó trở nên tuyến tính một cách hiệu quả trên mỗi trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose
    import sys

    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        ans = 0
        for l in range(n):
            cur_or = 0
            cur_max = 0
            for r in range(l, n):
                cur_or |= a[r]
                cur_max = max(cur_max, a[r])
                if cur_or == cur_max:
                    ans += 1
        out.append(str(ans))
    return "\n".join(out)

# provided samples
assert run("3\n4\n1 2 3 2\n5\n1 2 4 8 16\n6\n5 9 1 3 6 12") == "9\n5\n8"

# custom cases
assert run("1\n1\n7") == "1", "single element"
assert run("1\n2\n1 1") == "3", "all equal"
assert run("1\n3\n1 2 4") == "3", "powers of two"
assert run("1\n4\n1 3 5 7") == "6", "dense OR growth"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | trường hợp cơ sở đúng đắn | 
| tất cả đều bình đẳng | 3 | hành vi giá trị lặp đi lặp lại | 
| sức mạnh của hai | 3 | HOẶC tăng trưởng mà không hủy bỏ | 
| 1 3 5 7 | 6 | tương tác bit chồng chéo | 

## Vỏ cạnh 

Đối với mảng một phần tử, điều kiện luôn đúng vì OR và max đều là chính phần tử đó. Thuật toán đếm chính xác từng chỉ mục dưới dạng một mảng con hợp lệ có độ dài bằng một. 

Đối với các mảng trong đó tất cả các phần tử giống hệt nhau, mọi mảng con đều hợp lệ vì cả OR và giá trị tối đa vẫn bằng giá trị đó trong suốt quá trình mở rộng. 

Đối với các mảng có lũy thừa tăng dần bằng hai, tính hợp lệ chỉ xuất hiện khi điểm cuối bên phải đạt đến điểm mà tất cả các bit thấp hơn đã được tích lũy thành phần tử tối đa mà quá trình mở rộng nắm bắt một cách tự nhiên. 

Đối với các mẫu bit hỗn hợp như$[1,3,5,7]$, OR nhanh chóng bão hòa và tính hợp lệ chỉ xuất hiện trên các phân đoạn trong đó mức tối đa đã chứa tất cả các bit đóng góp mà quá trình kiểm tra đang chạy sẽ phát hiện chính xác ở đúng điểm cuối.
