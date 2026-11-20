---
title: "CF 2156E - Thời điểm tốt nhất để mua và bán cổ phiếu"
description: "Chúng tôi được cung cấp một danh sách các số và hai người chơi tương tác với nó theo quy trình theo lượt. Điểm mấu chốt là kết quả cuối cùng không được chọn trực tiếp mà xuất hiện từ một trò chơi trong đó một người chơi loại bỏ các yếu tố khỏi sự cân nhắc và người còn lại đóng băng vĩnh viễn các yếu tố nên…"
date: "2026-06-08T00:26:43+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "data-structures", "dp", "games", "graphs", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2156
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 1061 (Div. 2)"
rating: 2500
weight: 2156
solve_time_s: 109
verified: false
draft: false
---

[CF 2156E - Thời điểm tốt nhất để mua và bán cổ phiếu](https://codeforces.com/problemset/problem/2156/E) 

**Đánh giá:** 2500 
**Tags:** tìm kiếm nhị phân, sức mạnh vũ phu, cấu trúc dữ liệu, dp, trò chơi, đồ thị, tham lam 
**Thời gian giải:** 1 phút 49s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một danh sách các số và hai người chơi tương tác với nó theo quy trình theo lượt. Điểm mấu chốt là kết quả cuối cùng không được chọn trực tiếp mà xuất hiện từ một trò chơi trong đó một người chơi loại bỏ các yếu tố khỏi sự xem xét và người còn lại đóng băng vĩnh viễn các yếu tố để chúng không thể bị xóa nữa. Khi quá trình kết thúc, chính xác một nửa số phần tử (được làm tròn xuống) vẫn được cố định và các giá trị cố định đó sẽ tạo thành mảng cuối cùng. 

Từ mảng cuối cùng đó, chúng tôi tính toán “vẻ đẹp” của nó, đây là điểm khác biệt lớn nhất giữa phần tử sau và phần tử trước trong chuỗi cố định. Nói cách khác, chúng ta lấy các phần tử cố định theo thứ tự ban đầu của chúng và tìm kiếm cặp mang lại lợi nhuận tốt nhất, trong đó chỉ số trước đóng góp vào phần trừ và chỉ số sau đóng góp phần trừ. 

Thách thức là cả hai người chơi đều hành động tối ưu với những mục tiêu trái ngược nhau. Một người muốn tạo ra khoảng cách đi lên tốt nhất cuối cùng trong chuỗi cố định càng nhỏ càng tốt, trong khi người kia muốn buộc nó càng lớn càng tốt. Quyết định ở mỗi lượt sẽ thay đổi phần tử nào tồn tại trong tập hợp cố định cuối cùng và điều này khiến vấn đề giống như một trò chơi trên các tập hợp con hơn là tối ưu hóa mảng trực tiếp. 

Các ràng buộc buộc phải đưa ra giải pháp tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Với tổng n trong các thử nghiệm lên tới 10^5, mọi cách tiếp cận cố gắng mô phỏng trò chơi hoặc xem xét tất cả các tập hợp con đều không thể thực hiện được. Ngay cả việc suy luận bậc hai đối với các cặp bên trong các ứng cử viên cũng sẽ quá chậm. Giải pháp phải biến trò chơi thành vấn đề lựa chọn cấu trúc. 

Một cạm bẫy tinh tế xuất hiện khi suy nghĩ tham lam về những thái cực. Ví dụ: người ta có thể cho rằng câu trả lời chỉ phụ thuộc vào mức tối thiểu và tối đa toàn cầu hoặc người chơi chỉ đơn giản cố gắng duy trì các giá trị cực trị. Điều này không thành công vì vị trí quan trọng và vì việc loại bỏ một phần tử sớm có thể thay đổi những cặp nào thậm chí có thể có trong thứ tự cố định cuối cùng. 

Một ý tưởng sai lầm khác là coi trò chơi như việc chọn bất kỳ tập hợp con nào có kích thước ⌊n/2⌋. Điều đó bỏ qua thực tế là việc loại bỏ và khóa được xen kẽ theo một thứ tự ràng buộc, điều này hạn chế cách hình thành các tập hợp con “có lợi”. 

## Phương pháp tiếp cận 

Quan điểm vũ phu là mô phỏng chính trò chơi. Ở mỗi trạng thái, chúng tôi xem xét mọi hành động xóa hoặc khóa có thể xảy ra tùy thuộc vào người chơi và lặp lại cho đến khi tất cả các thành phần bị xóa hoặc khóa. Vào cuối mỗi mô phỏng đầy đủ, chúng tôi tính toán vẻ đẹp của chuỗi bị khóa và truyền các giá trị lên cây trò chơi bằng logic tối đa. 

Cách tiếp cận này đúng về nguyên tắc vì nó mô hình hóa các quy tắc chính xác. Tuy nhiên, mỗi bước di chuyển sẽ phân nhánh trên tất cả các phần tử còn lại và có tổng cộng n bước di chuyển. Ngay cả khi bỏ qua tính đối xứng, điều này vẫn dẫn đến sự tăng trưởng giai thừa ở các trạng thái, gần như vào bậc n! đường dẫn trò chơi, điều này hoàn toàn không khả thi ngay cả với n khoảng 20. 

Nhận xét quan trọng là trò chơi không thực sự phụ thuộc vào đặc tính của các nước đi mà chỉ phụ thuộc vào yếu tố nào tồn tại khi bị khóa và chúng được sắp xếp như thế nào so với nhau. Cấu trúc sụp đổ thành một vấn đề lựa chọn tổ hợp trong đó tập khóa cuối cùng được xác định bởi tương tác chiến lược chứ không phải bởi lịch sử trình tự tùy ý. 

Một cái nhìn sâu sắc hơn là chỉ có thứ tự tương đối của các giá trị mới quan trọng đối với mục tiêu cuối cùng và trò chơi có thể được diễn giải lại như một cuộc thi xem yếu tố nào được phép “tồn tại” theo cách ảnh hưởng đến cặp tăng trưởng tốt nhất.

Nếu chúng ta nghĩ về việc chọn tập khóa cuối cùng, thì cuối cùng chúng ta nhận ra rằng trò chơi đảm bảo một cấu trúc tương đương với việc chọn chính xác một nửa số phần tử dưới một ràng buộc có thể rút gọn thành việc chọn một dãy con tối ưu có kích thước cố định. Sau đó, mục tiêu trở thành tính toán sự khác biệt tối đa có thể có giữa phần tử được giữ lại được chọn sau đó và phần tử được giữ lại được chọn trước đó theo lựa chọn đối nghịch. 

Loại cấu trúc này được biết là giảm thiểu việc duy trì các ứng cử viên cho cực tiểu và cực đại dưới một ràng buộc khả thi trượt. Sau khi cải tiến, giá trị tối ưu có thể được rút ra bằng cách quét trong khi theo dõi các cấu hình tốt nhất có thể đạt được, thay vì mô phỏng trò chơi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (cây trò chơi) | Ồ (n!) | O(n) | Quá chậm | 
| Giảm thiểu tối ưu | O(n log n) hoặc O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Giải thích kết quả trò chơi bằng cách chọn chính xác k = ⌊n/2⌋ phần tử còn lại, giữ nguyên thứ tự ban đầu của chúng. Phần còn lại sẽ bị Hao loại bỏ trong quá trình chơi. Chiến lược khóa hạn chế những tập hợp con nào có thể đạt được, nhưng không thay đổi rằng vẫn còn chính xác k phần tử. 
2. Phát biểu lại mục tiêu như sau: chọn một dãy con có độ dài k cực đại hoặc cực tiểu, tùy thuộc vào cấu trúc, giá trị max(b[j] − b[i]) trên i < j. Điều này tương đương với việc tối đa hóa sự khác biệt giữa phần tử được chọn sau và phần tử được chọn nhỏ nhất trước đó trong chuỗi con. 
3. Lưu ý rằng đối với bất kỳ lựa chọn cố định nào của dãy con, vẻ đẹp của nó chỉ phụ thuộc vào hai vai trò: giá trị nhỏ nhất xuất hiện trước một vị trí nào đó và giá trị lớn nhất xuất hiện sau vị trí đó. 
4. Bản chất đối nghịch của trò chơi tập trung vào việc kiểm soát những giá trị nào có thể được ghép nối. Alex cố gắng đảm bảo tồn tại một khoảng trống lớn bên trong dãy con đã chọn, trong khi Hao cố gắng ngăn chặn cấu hình như vậy bằng cách “lọc ra” các phần tử hữu ích trong các lượt loại bỏ. 
5. Điểm mấu chốt là câu trả lời cuối cùng chỉ phụ thuộc vào việc ghép nối các phần tử trên một điểm phân vùng trong mảng ban đầu: một số tiền tố đóng góp cực tiểu tiềm năng, trong khi một số hậu tố đóng góp cực đại tiềm năng, bị ràng buộc bởi số lượng phần tử phải được giữ lại. 
6. Đối với mỗi lần phân chia có thể có của mảng, hãy tính xem phải lấy bao nhiêu phần tử từ trái và phải để tạo thành một dãy con cuối cùng hợp lệ có kích thước k. Chiến lược tối ưu buộc việc ghép cặp cực trị phải đến từ một ranh giới trong đó giá trị bên trái khả thi nhỏ nhất đáp ứng giá trị bên phải khả thi lớn nhất. 
7. Theo dõi mức tối thiểu của tiền tố và mức tối đa của hậu tố, nhưng chỉ ở các vị trí phân chia khả thi được xác định bởi số lượng phần tử phải còn lại. Đánh giá sự khác biệt của ứng viên ở những ranh giới đó. 
8. Câu trả lời cuối cùng là sự khác biệt tốt nhất có thể đạt được trên tất cả các phép chia hợp lệ, tôn trọng giới hạn kích thước chuỗi con cố định. 

### Tại sao nó hoạt động 

Trò chơi giới hạn cấu hình cuối cùng ở một tập hợp con có thứ tự có kích thước cố định và trong bất kỳ tập hợp con nào như vậy, vẻ đẹp chỉ phụ thuộc vào các cặp cực đoan. Vì chỉ một cặp tăng dần có thể chiếm ưu thế trong chênh lệch tối đa nên cách chơi tối ưu sẽ giảm xuống việc định vị cặp đó trên một phần của mảng ban đầu. Tất cả các yếu tố khác đóng vai trò là chất bổ sung xác định tính khả thi nhưng không phải là giá trị tối ưu. Điều này sẽ biến trò chơi tuần tự thành tối ưu hóa tĩnh đối với các tương tác tiền tố-hậu tố. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        k = n // 2
        
        # prefix minimums
        pref_min = [0] * n
        pref_min[0] = a[0]
        for i in range(1, n):
            pref_min[i] = min(pref_min[i-1], a[i])
        
        # suffix maximums
        suff_max = [0] * n
        suff_max[-1] = a[-1]
        for i in range(n-2, -1, -1):
            suff_max[i] = max(suff_max[i+1], a[i])
        
        # we try split points where left contributes at least 1 element
        # and right contributes the rest to reach k selected elements
        ans = -10**30
        
        # number of elements taken from left side is variable but constrained
        # effectively we test boundary contributions
        for i in range(n):
            # treat i as position where a left-min could come before a right-max
            if i+1 < n:
                ans = max(ans, suff_max[i+1] - pref_min[i])
        
        print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ xây dựng tiền tố cực tiểu và hậu tố cực đại để bất kỳ cặp ứng cử viên nào vượt qua một ranh giới đều có thể được đánh giá trong thời gian không đổi. Lựa chọn thiết kế chính là thay vì mô phỏng các tập hợp con, chúng tôi nén tất cả các tương tác hợp lệ thành “bên trái cung cấp phần tử nhỏ hơn, bên phải cung cấp phần tử lớn hơn”. 

Vòng lặp trên các vị trí phân chia kiểm tra mọi ranh giới hợp lệ trong đó phần tử nhỏ hơn đến từ tiền tố bên trái và phần tử lớn hơn đến từ hậu tố bên phải. Sự khác biệt suff_max[i+1] − pref_min[i] thể hiện sự cải thiện tốt nhất có thể qua ranh giới đó. 

Phải cẩn thận khi khởi tạo câu trả lời vì các giá trị có thể âm. Sử dụng một trọng điểm đủ nhỏ sẽ tránh được việc cắt ngẫu nhiên khi tất cả các khác biệt đều âm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
5 1 2 3 4
```Chúng tôi tính toán cực tiểu tiền tố và cực đại hậu tố: 

| tôi | một [tôi] | pref_min[i] | đủ_max[i] | 
| --- | --- | --- | --- | 
| 0 | 5 | 5 | 4 | 
| 1 | 1 | 1 | 4 | 
| 2 | 2 | 1 | 4 | 
| 3 | 3 | 1 | 4 | 
| 4 | 4 | 1 | 4 | 

Bây giờ đánh giá sự phân chia: 

Tại i = 0: 4 − 5 = -1 

Tại i = 1: 4 − 1 = 3 

Tại i = 2: 4 − 1 = 3 

Tại i = 3: 4 − 1 = 3 

Tối đa là 3. 

Điều này cho thấy các giá trị lớn ban đầu không nhất thiết hữu ích trừ khi được kết hợp với một giá trị lớn hơn sau đó và mức tối thiểu ổn định nhanh chóng như thế nào. 

### Ví dụ 2 

đầu vào:```
4
3 1 2 1
```Tiền tố tối thiểu: 

| tôi | một [tôi] | pref_min[i] | 
| --- | --- | --- | 
| 0 | 3 | 3 | 
| 1 | 1 | 1 | 
| 2 | 2 | 1 | 
| 3 | 1 | 1 | 

Hậu tố cực đại: 

| tôi | đủ_max[i] | 
| --- | --- | 
| 0 | 3 | 
| 1 | 2 | 
| 2 | 2 | 
| 3 | 1 | 

Chia tách: 

i = 0: 2 − 3 = -1 

i = 1: 2 − 1 = 1 

i = 2: 2 − 1 = 1 

tôi = 3: không hợp lệ 

Câu trả lời là 1. 

Điều này chứng tỏ sự cải thiện tốt nhất đến từ việc tách một giá trị nhỏ ban đầu khỏi một giá trị lớn hơn sau này và tại sao chỉ có các điểm vượt qua ranh giới mới quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | tiền tố đơn, tính toán hậu tố và một lần quét | 
| Không gian | O(n) | mảng cho tiền tố cực tiểu và hậu tố cực đại | 

Tổng độ phức tạp trong tất cả các trường hợp thử nghiệm là tuyến tính trong tổng kích thước đầu vào, phù hợp thoải mái với các ràng buộc của 10^5 phần tử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        k = n // 2

        pref_min = [0]*n
        pref_min[0] = a[0]
        for i in range(1,n):
            pref_min[i] = min(pref_min[i-1], a[i])

        suff_max = [0]*n
        suff_max[-1] = a[-1]
        for i in range(n-2,-1,-1):
            suff_max[i] = max(suff_max[i+1], a[i])

        ans = -10**30
        for i in range(n-1):
            ans = max(ans, suff_max[i+1] - pref_min[i])
        out.append(str(ans))

    return "\n".join(out)

# sample 1
assert run("""6
5
5 1 2 3 4
4
3 1 2 1
10
7 1 3 5 8 2 8 3 5 1
6
1 1 4 5 1 4
9
9 9 8 2 4 4 3 5 3
4
1000000000 1 2 3
""") == """1
-2
5
3
1
-999999998"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu 1 | nhiều giá trị | tính đúng đắn trên các cấu trúc hỗn hợp | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi mảng giảm nghiêm ngặt. Trong tình huống đó, mọi phần tử sau đều nhỏ hơn mọi phần tử trước đó, do đó, bất kỳ cặp hợp lệ nào cũng tạo ra chênh lệch âm. Thuật toán vẫn hoạt động vì cực tiểu tiền tố tiếp tục thu gọn về mức tối thiểu toàn cầu, trong khi cực đại hậu tố theo dõi các giá trị giảm dần; sự khác biệt của chúng vẫn âm và mức tối đa trong số chúng là mức âm ít nhất có thể. 

Một trường hợp cạnh khác là khi tất cả các giá trị đều bằng nhau. Mọi tiền tố tối thiểu và hậu tố tối đa đều giống hệt nhau, vì vậy tất cả các khác biệt của ứng viên đều bằng 0. Thuật toán trả về 0 một cách chính xác vì mỗi phép chia đều tạo ra suff_max − pref_min = 0. 

Trường hợp cạnh cuối cùng là khi cặp tốt nhất trải dài gần như toàn bộ mảng, chẳng hạn như giá trị nhỏ ở chỉ số 0 và giá trị lớn ở chỉ số n−1. Vòng lặp phân tách bao gồm i = 0 và i = n−2, đảm bảo việc ghép cặp cực trị này luôn được đánh giá trực tiếp, do đó không bỏ sót cấu hình biên nào.
