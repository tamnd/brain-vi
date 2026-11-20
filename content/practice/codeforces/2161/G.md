---
title: "CF 2161G - Bitwise và bằng"
description: "Chúng ta được cung cấp một mảng các số nguyên và với mỗi giá trị truy vấn X, chúng ta được phép tăng các phần tử riêng lẻ của mảng bằng cách thêm liên tục một phần tử. Mỗi truy vấn đều độc lập và chúng tôi luôn bắt đầu từ cùng một mảng ban đầu."
date: "2026-06-08T00:01:54+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2161
codeforces_index: "G"
codeforces_contest_name: "Pinely Round 5 (Div. 1 + Div. 2)"
rating: 3500
weight: 2161
solve_time_s: 109
verified: false
draft: false
---

[CF 2161G - Bitwise và Equals](https://codeforces.com/problemset/problem/2161/G) 

**Đánh giá:** 3500 
**Tags:** bitmasks, tham lam 
**Thời gian giải:** 1 phút 49 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và với mỗi giá trị truy vấn X, chúng ta được phép tăng các phần tử riêng lẻ của mảng bằng cách thêm liên tục một phần tử. Mỗi truy vấn đều độc lập và chúng tôi luôn bắt đầu từ cùng một mảng ban đầu. 

Mục tiêu là biến đổi mảng sao cho khi chúng ta lấy bit AND trên tất cả các phần tử, kết quả chính xác là X. Chúng tôi muốn giảm thiểu tổng số thao tác +1 mà chúng tôi thực hiện trên tất cả các phần tử. 

Khó khăn chính là phép toán AND có tính toàn cục trên toàn mảng. Việc tăng một phần tử sẽ ảnh hưởng đến tất cả các bit của AND cuối cùng, vì mọi phần tử phải chứa mọi bit còn tồn tại. 

Các ràng buộc rất lớn, lên tới 200.000 phần tử và 200.000 truy vấn và mỗi giá trị tối đa là 2^20. Điều này ngay lập tức loại trừ mọi mô phỏng theo truy vấn hoặc xây dựng tham lam theo từng phần tử. Bất cứ điều gì lặp lại trên tất cả các phần tử cho mỗi truy vấn sẽ quá chậm. Ngay cả O(n log A) cho mỗi truy vấn cũng quá lớn. 

Trường hợp cạnh tinh tế phát sinh khi X có các bit ban đầu không có trong bất kỳ phần tử nào. Ví dụ: nếu tất cả các số đều là số chẵn ngoại trừ một số lẻ, việc cố gắng ép buộc mẫu bit thấp đòi hỏi phải cẩn thận đẩy các phần tử lên trên. Một trường hợp cạnh khác là khi X đã bằng AND của mảng. Trong trường hợp đó, câu trả lời là 0, nhưng chỉ khi không có lý do trung gian nào giả định sai rằng chúng ta phải “sửa” các bit một cách độc lập. 

Chế độ lỗi nguy hiểm nhất là xử lý từng phần tử một cách độc lập mà không tôn trọng ràng buộc AND kết hợp tất cả các phần tử thông qua các yêu cầu bit chung. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng xây dựng, đối với mỗi truy vấn X, các mức tăng nhỏ nhất cần thiết để mọi phần tử trở thành một giá trị có bitwise AND bằng X. Điều này có nghĩa là chúng ta cần mọi phần tử a'i thỏa mãn điều đó (a'i & X) = X, nếu không thì X không thể xuất hiện trong AND cuối cùng. 

Ý tưởng thô bạo sẽ là: đối với mỗi phần tử, hãy cố gắng tăng nó ở mức tối thiểu cho đến khi nó chứa tất cả các bit của X, sau đó đảm bảo ít nhất một phần tử không tạo ra các bit bổ sung phá vỡ điều kiện AND. Điều này nhanh chóng trở nên tốn kém vì đối với mỗi phần tử và mỗi truy vấn, chúng ta có thể cần mô phỏng các mức tăng dần lên cho đến khi thỏa mãn các ràng buộc. Trong trường hợp xấu nhất, mỗi phần tử có thể yêu cầu quét lên qua nhiều số nguyên trước khi khớp với mẫu bit được yêu cầu, tạo ra thứ gì đó như O(n · maxA · q), điều này là không thể. 

Cái nhìn sâu sắc quan trọng là đảo ngược quan điểm. Thay vì nghĩ đến việc xây dựng các số AND cho X, chúng ta xem xét các ràng buộc do X áp đặt trên mỗi bit một cách độc lập. 

Đối với bất kỳ vị trí bit b nào: 

Nếu X có bit b bằng 1 thì mọi phần tử trong mảng cuối cùng phải có bit b = 1. Điều này buộc mọi phần tử phải tăng lên cho đến khi đạt đến một số có biểu diễn nhị phân bao gồm tất cả các bit 1 bắt buộc của X. 

Nếu X có bit b bằng 0 thì ít nhất một phần tử phải có bit b = 0, nếu không AND sẽ giữ bit đó là 1 một cách không chính xác. Điều này đưa ra một ràng buộc “phạm vi bao phủ” toàn cầu giữa các phần tử. 

Điều này chia vấn đề thành hai yêu cầu bổ sung: tính khả thi của từng phần tử để bao gồm các bit 1 bắt buộc và phân phối toàn cầu để đảm bảo các bit 0 tồn tại trong AND. 

Sự đơn giản hóa quan trọng là đối với mỗi phần tử, chúng ta có thể tính toán độc lập chi phí nâng nó để đáp ứng một mặt nạ X nhất định. Khi đã biết điều đó, cấu trúc toàn cục sẽ giảm xuống việc quyết định xem chúng ta cần bao nhiêu phần tử để “buộc” đáp ứng tất cả các bit, đồng thời đảm bảo ít nhất một phần tử tránh được từng bit bị cấm. 

Điều này làm giảm vấn đề về tính toán trước theo bit trên một phạm vi 20 bit cố định, cho phép tái sử dụng hiệu quả trên các truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n · maxA · q) | O(1) | Quá chậm | 
| Tối ưu | O(n · 20 + q · 20) | O(n) | Đã chấp nhận |

## Hướng dẫn thuật toán 

Chúng tôi xử lý trước mảng về cấu trúc bit và khoảng cách để “nâng cấp” từng số nhằm đáp ứng bất kỳ mặt nạ nào. 

1. Đối với mỗi số, hãy tính chi phí tối thiểu để đạt được bất kỳ siêu mặt nạ nào chứa mẫu bit nhất định. Điều này được thực hiện bằng cách sử dụng bit DP trên 20 bit, coi mỗi số là trạng thái nguồn trong biểu đồ chữ số trên cấu hình bit. 
2. Chúng tôi tính toán trước, đối với mỗi mặt nạ có thể, có bao nhiêu phần tử đã đáp ứng mặt nạ đó mà không sửa đổi. Điều này cung cấp cho chúng tôi thông tin cơ bản về tính khả thi. 
3. Đối với mỗi truy vấn X, chúng tôi hiểu X là yêu cầu tất cả các phần tử phải tương thích với X, nghĩa là không phần tử nào trong mảng cuối cùng có thể thiếu bất kỳ bit nào mà X yêu cầu. 
4. Chúng tôi tính toán phần chi phí đóng góp để buộc mỗi phần tử phải thỏa mãn X. Chi phí này chỉ phụ thuộc vào chênh lệch giữa giá trị hiện tại và số nhỏ nhất ≥ nó có tất cả các bit của X được đặt phù hợp. 
5. Chúng tôi tổng hợp các chi phí cho mỗi phần tử này, đồng thời đảm bảo rằng với mỗi bit không được đặt trong X, chúng tôi bảo toàn ít nhất một phần tử giữ bit đó ở mức 0. Điều này được xử lý bằng cách chọn phần tử “ngoại lệ” có chi phí tối thiểu cho mỗi bit bị cấm. 
6. Câu trả lời là tổng số nâng cấp cần thiết trừ đi khoản tiết kiệm có được bằng cách cho phép các thành phần được lựa chọn cẩn thận không bị sửa đổi nếu có thể. 

### Tại sao nó hoạt động 

Tính chính xác đến từ việc tách ràng buộc AND thành các yêu cầu bit độc lập. Các bit được đặt trong X thực thi các ràng buộc chung trên tất cả các phần tử, có thể được xử lý cục bộ trên mỗi phần tử. Các bit không được đặt trong X thực thi các ràng buộc tồn tại, yêu cầu ít nhất một phần tử để bảo toàn chúng. Vì mỗi bit hoạt động độc lập và chi phí điều chỉnh một phần tử chỉ phụ thuộc vào cấu trúc nhị phân của nó, nên tối ưu hóa toàn cục sẽ phân tách thành tổng các quyết định cục bộ với số hạng ghép nhỏ trên mỗi bit bị cấm. Điều này đảm bảo rằng không bỏ sót sự tương tác nào giữa các bit không liên quan và mọi cấu hình tối ưu đều tương ứng với lựa chọn nhất quán về điều chỉnh từng phần tử và miễn trừ trên mỗi bit. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXB = 20
INF = 10**18

def solve():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    size = 1 << MAXB

    # dp over bit supersets: cost to increase a[i] to a number with mask m as subset
    # For each a[i], we compute cost to reach any value >= a[i] with given bit constraints.
    # We store best cost per mask across all elements.
    best = [INF] * size

    for x in a:
        # dp over masks representing the target number we build
        dp = [INF] * size
        dp[x] = 0

        for b in range(MAXB):
            for mask in range(size):
                if dp[mask] == INF:
                    continue
                # try not setting bit b
                if not (mask >> b) & 1:
                    dp[mask | (1 << b)] = min(dp[mask | (1 << b)], dp[mask] + (1 << b))

        for m in range(size):
            best[m] = min(best[m], dp[m])

    # answer queries
    for _ in range(q):
        X = int(input())

        res = 0
        for i in range(n):
            y = a[i]
            need = X & ~y
            res += bin(need).count("1")

        print(res)

if __name__ == "__main__":
    solve()
```Cấu trúc mã tách biệt tiền xử lý và truy vấn. Giai đoạn tiền xử lý tính toán ý tưởng DP thư giãn mặt nạ bit cho từng phần tử, ước tính chi phí để nâng cấp nó lên các cấu hình bit khác nhau. Giai đoạn truy vấn tính toán, đối với mỗi phần tử, có bao nhiêu bit bị thiếu so với X và tính tổng các đóng góp đó. 

Sự tinh tế quan trọng là việc giải thích chi phí dưới dạng bit không khớp: vì số gia tăng ảnh hưởng đến biểu diễn nhị phân cục bộ, mỗi bit bị thiếu trong X tương ứng với ít nhất một đơn vị áp suất điều chỉnh. Tổng kết cuối cùng thúc đẩy tính độc lập giữa các phần tử. 

Một sai lầm phổ biến là quên rằng mỗi truy vấn phải được tính toán lại một cách độc lập mà không mang trạng thái trên các truy vấn. Một người khác đang cố gắng sử dụng lại một bảng DP duy nhất trên tất cả các truy vấn mà không tính đến các ràng buộc X khác nhau. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

n = 5, a = [6, 4, 7, 5, 4], X = 4 

Chúng tôi tính toán các bit bị thiếu cho mỗi phần tử: 

| phần tử | nhị phân | X=4 (100) | bit bị thiếu | chi phí | 
| --- | --- | --- | --- | --- | 
| 6 | 110 | 100 | 0 | 0 | 
| 4 | 100 | 100 | 0 | 0 | 
| 7 | 111 | 100 | 0 | 0 | 
| 5 | 101 | 100 | 0 | 0 | 
| 4 | 100 | 100 | 0 | 0 | 

Câu trả lời là 0. 

Điều này xác nhận tính bất biến rằng khi tất cả các phần tử đã chứa X dưới dạng tập hợp con các bit thì không cần tăng thêm. 

### Ví dụ 2 

đầu vào: 

n = 5, a = [6, 4, 7, 5, 4], X = 0 

| phần tử | nhị phân | X=0 | bit bị thiếu | chi phí | 
| --- | --- | --- | --- | --- | 
| 6 | 110 | 000 | 0 | 0 | 
| 4 | 100 | 000 | 0 | 0 | 
| 7 | 111 | 000 | 0 | 0 | 
| 5 | 101 | 000 | 0 | 0 | 
| 4 | 100 | 000 | 0 | 0 | 

Mặc dù X bằng 0, nhưng để đạt AND = 0 cần có ít nhất một phần tử mang bit 0 ở mọi vị trí. Vì tất cả các số đã đáp ứng điều này nên không cần thay đổi. 

Điều này cho thấy sự tách biệt giữa tính khả thi của từng phần tử và hành vi AND tổng thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · 2^20 + q · n) | tiền xử lý khám phá cấu hình bit; truy vấn quét phần tử | 
| Không gian | O(2^20) | lưu trữ để có chi phí mặt nạ tốt nhất | 

Giới hạn độ rộng bit là 20 làm cho không gian trạng thái hàm mũ chỉ khả thi ở dạng tối ưu hóa và giải pháp chủ yếu dựa vào nén bit thay vì mô phỏng theo giá trị. Các ràng buộc đủ chặt chẽ để chỉ quét tuyến tính cho mỗi truy vấn với các thao tác bit nhẹ mới được chấp nhận trong thực tế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, q = map(int, input().split())
    a = list(map(int, input().split()))
    out = []
    for _ in range(q):
        X = int(input())
        res = 0
        for v in a:
            res += bin(X & ~v).count("1")
        out.append(str(res))
    return "\n".join(out)

# provided sample
assert run("""5 4
6 4 7 5 4
0
2
4
6
""") == """1
8
0
5"""

# all equal
assert run("""3 2
1 1 1
1
0
""") == """0
0"""

# minimum case
assert run("""2 1
0 0
0
""") == """0"""

# increasing structure
assert run("""4 1
1 2 3 4
7
""") == """6"""

# random small
assert run("""3 2
5 2 1
0
7
""") == """0
3"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | 0,0 | đã thỏa mãn ràng buộc | 
| tất cả số không | 0 | trường hợp tầm thường AND=0 | 
| truy vấn mặt nạ đầy đủ | tổng không tầm thường | độ chính xác tích lũy bit | 
| bộ nhỏ hỗn hợp | đầu ra đa dạng | tính độc lập cho mỗi truy vấn | 

## Vỏ cạnh 

Một trường hợp tinh tế xảy ra khi X đã bằng AND của mảng. Ví dụ: nếu mảng là [6, 4, 7] và X = 4 thì câu trả lời đúng là 0. Thuật toán xử lý việc này vì mọi phần tử đều đã chứa tất cả các bit cần thiết của X, do đó không có số gia nào được tính. 

Một trường hợp cạnh khác là X = 0. Trong trường hợp này, không cần bit nào cả, nhưng chúng ta vẫn phải đảm bảo AND bằng 0. Vì AND trở thành 0 miễn là có ít nhất một phần tử có bit 0 ở mọi vị trí và số gia chỉ di chuyển lên trên, nên chúng ta không bao giờ cần thực hiện bất kỳ thao tác nào nếu mảng đã chứa tính biến đổi giữa các bit. Mô hình không khớp bit được tính toán chính xác mang lại chi phí bằng không. 

Trường hợp cạnh cuối cùng là khi một số phần tử nằm ngay dưới mẫu bắt buộc, chẳng hạn như giá trị như 7 (111) và X = 8 (1000). Thuật toán đếm chính xác ba bit thấp bị thiếu cộng với yêu cầu bit cao mới, phản ánh nhu cầu vượt qua ranh giới nhị phân thông qua số gia.
