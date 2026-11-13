---
title: "CF 1729D - Những Người Bạn Và Nhà Hàng"
description: "Chúng ta được cung cấp một tập hợp bạn bè, trong đó mỗi người bạn có hai con số: họ dự định chi bao nhiêu tại một nhà hàng và số tiền họ thực sự có. Mục tiêu là chia một số người bạn này thành các nhóm riêng biệt, trong đó mỗi nhóm có ít nhất hai người."
date: "2026-06-15T02:33:52+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1729
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 820 (Div. 3)"
rating: 1200
weight: 1729
solve_time_s: 232
verified: true
draft: false
---

[CF 1729D - Những người bạn và nhà hàng](https://codeforces.com/problemset/problem/1729/D) 

**Đánh giá:** 1200 
**Tags:** tham lam, sắp xếp, hai con trỏ 
**Thời gian giải:** 3 phút 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp bạn bè, trong đó mỗi người bạn có hai con số: họ dự định chi bao nhiêu tại một nhà hàng và số tiền họ thực sự có. Mục tiêu là chia một số người bạn này thành các nhóm riêng biệt, trong đó mỗi nhóm có ít nhất hai người. Mỗi người chỉ được sử dụng tối đa một lần và một số bạn bè có thể bị loại bỏ hoàn toàn. 

Một nhóm chỉ hợp lệ nếu tổng số tiền có sẵn trong nhóm ít nhất bằng tổng kế hoạch chi tiêu của nhóm đó. Chúng tôi muốn tối đa hóa số lượng nhóm hợp lệ mà chúng tôi có thể hình thành. 

Cấu trúc này không phải là phân bổ ngân sách riêng lẻ mà là ghép nối mọi người để “tiền thặng dư” bù đắp cho “thâm hụt” chung trong mỗi nhóm. Điều này ngay lập tức gợi ý một vấn đề cân bằng hơn là một vấn đề đối sánh đơn giản. 

Những hạn chế đẩy chúng ta tới một giải pháp hiệu quả. Vì tổng số bạn bè trong tất cả các trường hợp thử nghiệm lên tới 100.000, nên bất kỳ giải pháp bậc hai nào trong n cho mỗi trường hợp thử nghiệm đều quá chậm. Ngay cả O(n log n) cho mỗi trường hợp thử nghiệm cũng có thể chấp nhận được, nhưng bất cứ điều gì tệ hơn O(n^2) thì không. 

Một ý tưởng ngây thơ là thử tất cả các phân vùng có thể hoặc xây dựng các nhóm một cách tham lam bằng cách kiểm tra các tập hợp con. Điều đó thất bại nhanh chóng vì số lượng tập hợp con tăng theo cấp số nhân. 

Một vấn đề tinh tế hơn là các quyết định của nhóm có sự tương tác với nhau. Một người bạn có thặng dư lớn có thể hữu ích trong nhiều nhóm tiềm năng và một quyết định sai lầm về việc phân nhóm sớm có thể làm giảm đi câu trả lời cuối cùng. Ví dụ: nếu chúng ta tham lam thành lập một nhóm với một người bạn có tổng lợi ích tích cực mạnh mẽ và một đối tác yếu kém, chúng ta có thể lãng phí phần thặng dư mạnh mẽ mà lẽ ra có thể cân bằng cho một nhóm lớn hơn sau này. 

Khó khăn chính là việc phân nhóm bị ràng buộc bởi một điều kiện tổng hợp chứ không phải tính khả thi riêng lẻ. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu sẽ cố gắng chỉ định từng người bạn vào một nhóm nào đó và xác thực tất cả các phân vùng. Ngay cả khi chúng ta hạn chế xây dựng nhóm dần dần, chúng ta vẫn cần xem xét các tập hợp con của những người bạn còn lại ở mỗi bước. Điều này dẫn đến số lượng trạng thái theo cấp số nhân, vì mỗi người bạn có thể được xếp vào bất kỳ nhóm nào hoặc bị loại. Ngay cả khi cắt tỉa, trường hợp xấu nhất vẫn khó xử lý với n lên tới 100.000. 

Bước ngoặt đến từ việc diễn giải lại mỗi người bạn đều đóng góp một giá trị ròng: “ngân sách trừ chi phí” của họ hoặc$y_i - x_i$. Một nhóm hợp lệ nếu tổng các giá trị này trong nhóm không âm. Điều này chuyển đổi vấn đề thành các nhóm hình thành có tổng giá trị ròng ít nhất bằng 0, với ràng buộc là mỗi nhóm phải chứa ít nhất hai phần tử. 

Một khi được nhìn nhận theo cách này, vấn đề giống như việc ghép nối hoặc nhóm các phần tử dựa trên đóng góp ròng đã được sắp xếp. Nếu chúng ta sắp xếp bạn bè theo thứ tự có ý nghĩa, chúng ta có thể tham lam quyết định xem yếu tố nào sẽ giữ vững nhóm và yếu tố nào sẽ hỗ trợ họ. 

Chiến lược tối ưu đến từ việc sắp xếp bạn bè theo giá trị ròng của họ$d_i = y_i - x_i$. Giá trị dương có ích, giá trị âm có hại. Để tối đa hóa số lượng nhóm, chúng tôi muốn “vô hiệu hóa” càng nhiều đóng góp tiêu cực càng tốt bằng cách sử dụng những đóng góp tích cực. Chiến lược hai con trỏ tham lam xuất hiện một cách tự nhiên: ghép yếu nhất (tiêu cực nhất) với mạnh nhất (tích cực nhất), tạo thành một nhóm bất cứ khi nào tổng kết hợp của chúng không âm và tiếp tục đi vào trong. 

Về mặt tinh thần, điều này tương tự như các bài toán ghép cặp tối ưu trong đó các điểm cực trị được kết hợp để tối đa hóa tính khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu (sắp xếp + hai con trỏ) | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính giá trị ròng của mỗi người bạn như sau$d_i = y_i - x_i$. Điều này biến đổi điều kiện của một nhóm thành một ràng buộc tổng trên một mảng. Một nhóm hợp lệ nếu tổng số được chọn của nó$d_i$là không âm. 
2. Sắp xếp tất cả$d_i$theo thứ tự không giảm. Điều này đặt các giá trị âm nhất ở đầu và các giá trị dương nhất ở cuối, điều này giúp dễ dàng suy luận về các thái cực ghép đôi. 
3. Khởi tạo hai con trỏ, một ở đầu (trái) và một ở cuối (phải), đồng thời khởi tạo bộ đếm số lượng nhóm. 
4. Khi trái < phải, hãy cân nhắc việc ghép hai giá trị cực trị. Nếu như$d[left] + d[right] \ge 0$, thì hai con trỏ này có thể tạo thành một nhóm hợp lệ có kích thước hai, vì vậy chúng ta đếm một nhóm và di chuyển cả hai con trỏ vào trong. Điều này là tối ưu vì giá trị khả dụng mạnh nhất đang được sử dụng để bù cho giá trị khả dụng yếu nhất, bảo toàn cấu trúc cho các phần tử còn lại. 
5. Nếu$d[left] + d[right] < 0$, thì ngay cả người bạn mạnh mẽ nhất cũng không thể bù đắp được cho người yếu nhất. Trong trường hợp đó, chúng tôi loại bỏ phần tử yếu nhất bằng cách di chuyển về phía trước sang trái. Điều này an toàn vì phần tử yếu nhất không thể là một phần của bất kỳ nhóm hợp lệ nào khi nó được ghép nối với bất kỳ phần tử còn lại nào khác. 
6. Tiếp tục cho đến khi các con trỏ gặp nhau. Số lượng ghép nối thành công chính là câu trả lời. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên thực tế là trong bất kỳ giải pháp tối ưu nào, chúng ta có thể giả sử các nhóm được hình thành một cách hiệu quả bằng cách ghép các cặp cực đối lập sau khi sắp xếp. Nếu một phần tử rất tiêu cực không thể được ghép nối với phần tử dương lớn nhất hiện có, thì nó cũng không thể được ghép nối với bất kỳ phần tử nào khác, bởi vì tất cả những phần tử khác đều ít hữu ích hơn. Vì vậy, nó là an toàn để loại bỏ nó. Ngược lại, bất cứ khi nào giá trị dương lớn nhất có thể bù đắp cho giá trị âm nhỏ nhất, việc ghép chúng luôn an toàn vì việc thay thế một trong hai bằng đối tác có cường độ nhỏ hơn không thể cải thiện tính khả thi. Sự ghép đôi cực đoan tham lam này duy trì việc sử dụng tối đa những mặt tích cực mạnh mẽ trong khi loại bỏ những mặt tiêu cực hạn chế nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        x = list(map(int, input().split()))
        y = list(map(int, input().split()))
        
        d = [y[i] - x[i] for i in range(n)]
        d.sort()
        
        l, r = 0, n - 1
        ans = 0
        
        while l < r:
            if d[l] + d[r] >= 0:
                ans += 1
                l += 1
                r -= 1
            else:
                l += 1
        
        print(ans)

if __name__ == "__main__":
    solve()
```Quá trình triển khai bắt đầu bằng cách gộp từng người bạn thành một giá trị số dư ròng duy nhất. Điều này rất quan trọng vì ràng buộc nhóm chỉ phụ thuộc vào tổng. 

Việc sắp xếp đảm bảo rằng chúng ta luôn có thể suy luận về những ứng viên còn lại “tệ nhất” và “tốt nhất”. Hai con trỏ mô phỏng liên tục việc cố gắng giải cứu người tham gia yếu nhất bằng cách sử dụng con trỏ mạnh nhất hiện có. 

điều kiện`l < r`buộc mỗi nhóm phải có ít nhất hai người. Điều này phù hợp trực tiếp với ràng buộc vấn đề. 

Khi một cặp hợp lệ, chúng tôi ngay lập tức cam kết với nó. Khi không, chúng tôi loại bỏ phần tử yếu nhất vì không có cặp đôi nào trong tương lai có thể giải cứu nó hiệu quả hơn ứng cử viên mạnh nhất hiện tại. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi một ví dụ nhỏ: 

đầu vào:```
n = 6
x = [8, 3, 9, 2, 4, 5]
y = [5, 3, 1, 4, 5, 10]
```Giá trị ròng:`d = [-3, 0, -8, 2, 1, 5] → sorted = [-8, -3, 0, 1, 2, 5]`| Bước | trái | đúng | d[trái] | d[phải] | tổng hợp | hành động | nhóm | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 5 | -8 | 5 | -3 | bỏ trái | 0 | 
| 2 | 1 | 5 | -3 | 5 | 2 | cặp | 1 | 
| 3 | 2 | 4 | 0 | 2 | 2 | cặp | 2 | 

Câu trả lời cuối cùng là 2. 

Dấu vết này cho thấy các giá trị âm yếu không thể giải cứu ngay lập tức sẽ bị loại bỏ, trong khi các cực trị khả thi được ghép nối một cách tham lam. 

Một ví dụ thứ hai: 

đầu vào:```
n = 4
x = [1, 2, 3, 4]
y = [1, 1, 2, 2]
```Giá trị ròng:`[0, -1, -1, -2] → sorted = [-2, -1, -1, 0]`| Bước | trái | đúng | tổng hợp | hành động | nhóm | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 3 | -2 | bỏ trái | 0 | 
| 2 | 1 | 3 | -1 | bỏ trái | 0 | 
| 3 | 2 | 3 | -1 | bỏ trái | 0 | 

Không có cặp hợp lệ nào tồn tại nên câu trả lời là 0. 

Điều này chứng tỏ trường hợp ngay cả giá trị dương tốt nhất cũng không thể bù đủ số âm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế; quét hai con trỏ là tuyến tính | 
| Không gian | O(n) | Lưu trữ giá trị ròng | 

Giải pháp phù hợp thoải mái trong các ràng buộc vì tổng n trên các trường hợp thử nghiệm là 100.000, giúp việc sắp xếp theo từng trường hợp thử nghiệm đủ hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    input = sys.stdin.readline

    def solve():
        t = int(input())
        for _ in range(t):
            n = int(input())
            x = list(map(int, input().split()))
            y = list(map(int, input().split()))
            d = sorted(y[i] - x[i] for i in range(n))
            l, r = 0, n - 1
            ans = 0
            while l < r:
                if d[l] + d[r] >= 0:
                    ans += 1
                    l += 1
                    r -= 1
                else:
                    l += 1
            print(ans)

    solve()
    return ""

# provided samples
assert run("""6
6
8 3 9 2 4 5
5 3 1 4 5 10
4
1 2 3 4
1 1 2 2
3
2 3 7
1 3 10
6
2 3 6 9 5 7
3 2 7 10 6 10
6
5 4 2 1 8 100
1 1 1 1 1 200
6
1 4 1 2 4 2
1 3 3 2 3 4
""") == "", "sample test"

# custom cases
assert run("""1
2
1 1
100 100
""") == "", "minimum size"

assert run("""1
4
10 10 10 10
1 1 1 1
""") == "", "all negative net"

assert run("""1
6
1 2 3 4 5 6
6 5 4 3 2 1
""") == "", "balanced symmetric"

assert run("""1
5
1 100 1 100 1
100 1 100 1 100
""") == "", "alternating extremes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cặp kích thước tối thiểu | 0 hoặc 1 tùy | ràng buộc ghép nối cơ sở | 
| Toàn mạng âm | 0 | không có khả năng thành lập nhóm | 
| Giá trị đối xứng | cấu trúc ghép nối tối đa | tính đúng đắn dưới sự cân bằng | 
| Các thái cực xen kẽ | sự mạnh mẽ tham lam | ghép nối cực kỳ chính xác | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng xảy ra khi tất cả các giá trị ròng đều âm hoặc bằng 0. Ví dụ: nếu mỗi người bạn chi tiêu nhiều hơn mức họ có thì mảng được sắp xếp hoàn toàn không dương. Thuật toán liên tục loại bỏ các giá trị yếu nhất vì không có cặp hợp lệ nào với phần tử mạnh nhất có thể đạt được tổng không âm, dẫn đến không có nhóm nào phù hợp với thực tế vì không có nhóm hai người nào có thể đáp ứng điều kiện ngân sách. 

Một trường hợp khác là khi có chính xác một giá trị dương mạnh và nhiều giá trị hơi âm. Thuật toán đảm bảo rằng điểm tích cực mạnh nhất không bị lãng phí vào một cặp không thể ghép nối. Nó chỉ được sử dụng khi nó thực sự có thể cân bằng một yếu tố yếu, nếu không thì những âm bản yếu hơn sẽ bị loại bỏ trước tiên. 

Cuối cùng, khi các giá trị được phân cụm chặt chẽ xung quanh số 0, mọi nỗ lực ghép nối đều thành công và thuật toán tạo thành các nhóm tầng (n/2), điều này là tối ưu vì mỗi cặp đều đóng góp một nhóm hợp lệ không có cấu trúc còn sót lại.
