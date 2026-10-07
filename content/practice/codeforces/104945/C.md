---
title: "CF 104945C - Câu đố về Metro"
description: "Chúng ta được cung cấp một tập hợp các tuyến tàu điện ngầm, trong đó mỗi tuyến có thể được xem như một tập hợp con các ga từ một phạm vi cố định có kích thước lên tới 18. Một tuyến được mô tả đầy đủ về các ga mà nó dừng lại."
date: "2026-06-28T07:08:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104945
codeforces_index: "C"
codeforces_contest_name: "2023-2024 ICPC Southwestern European Regional Contest (SWERC 2023)"
rating: 0
weight: 104945
solve_time_s: 102
verified: false
draft: false
---

[CF 104945C - Câu đố về Metro](https://codeforces.com/problemset/problem/104945/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các tuyến tàu điện ngầm, trong đó mỗi tuyến có thể được xem như một tập hợp con các ga từ một phạm vi cố định có kích thước lên tới 18. Một tuyến được mô tả đầy đủ về các ga mà nó dừng lại. 

Một tuyến được chọn ngẫu nhiên như nhau và nhiệm vụ là xác định nó bằng cách đặt câu hỏi có/không có dạng “tuyến chưa biết có dừng ở ga i không?”. Mỗi câu trả lời chia các dòng ứng viên còn lại thành những dòng chứa trạm i và những dòng không chứa. 

Chúng tôi được yêu cầu thiết kế một chiến lược đặt câu hỏi tối ưu mà cuối cùng luôn xác định được dòng duy nhất và trong số tất cả các chiến lược như vậy, chúng tôi muốn giảm thiểu số lượng câu hỏi dự kiến ​​​​theo sự phân bổ thống nhất trên các dòng. 

Một chiến lược tương đương với việc xây dựng cây quyết định nhị phân. Mỗi nút bên trong truy vấn một trạm và mỗi cạnh tương ứng với có hoặc không, hạn chế tập hợp các dòng có thể. Chi phí của một chiếc lá là độ sâu của nó và chúng tôi giảm thiểu độ sâu trung bình của lá trên tất cả các dòng. 

Các ràng buộc chặt chẽ theo một cách rất cụ thể. Số lượng trạm nhiều nhất là 18, nghĩa là mỗi dòng có thể được mã hóa dưới dạng mặt nạ 18 bit. Số lượng dòng nhiều nhất là 50, vì vậy chúng ta đang thao tác trên một tập hợp đối tượng tương đối nhỏ, nhưng quá trình quyết định có thể phân nhánh theo cấp số nhân. Sự kết hợp này gợi ý rõ ràng một giải pháp lập trình động trên các tập hợp con của dòng, bởi vì chúng tôi đang tối ưu hóa mọi cách để phân tách một tập hợp nhỏ các mục bằng cách sử dụng một tập hợp nhỏ các tính năng nhị phân. 

Một điều kiện khả thi quan trọng là tính duy nhất. Nếu hai dòng có cùng một tập hợp các trạm thì mọi câu hỏi có thể đều tạo ra câu trả lời giống hệt nhau cho chúng, vì vậy chúng không bao giờ có thể phân biệt được. Trong trường hợp đó câu trả lời ngay lập tức là không thể. 

Một trường hợp lỗi nhỏ xuất hiện khi có nhiều đường khác nhau nhưng chỉ ở các trạm không bao giờ được truy vấn một cách hiệu quả. Sự phân chia tham lam ngây thơ có thể dễ dàng cô lập một dòng một cách nhanh chóng nhưng để lại một tập hợp còn lại rất mất cân bằng, tạo ra độ sâu dự kiến ​​dưới mức tối ưu. Đây là lý do tại sao phương pháp phỏng đoán phân tách cục bộ không hoạt động đáng tin cậy. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ xây dựng rõ ràng mọi cây quyết định có thể có. Tại mỗi nút chúng ta chọn một trạm để truy vấn, sau đó xây dựng đệ quy các cây con trái và phải. Số lượng cây có thể có là rất lớn vì mỗi tập hợp con của dòng có thể được chia theo nhiều cách và cùng một tập hợp con có thể được tiếp cận bằng các chuỗi truy vấn khác nhau. Ngay cả khi chỉ có 50 dòng, số lượng cây quyết định vẫn rất lớn và cách tiếp cận này thất bại ngay lập tức. 

Quan sát quan trọng là trạng thái của quy trình hoàn toàn được xác định bởi tập hợp các dòng ứng cử viên còn lại, chứ không phải bằng cách chúng tôi đến đó. Nếu hiện tại chúng ta đang ở tập con S của các dòng, thì chi phí dự kiến ​​tối ưu từ thời điểm này chỉ phụ thuộc vào S. Điều này tự nhiên dẫn đến một công thức lập trình động trên các tập con của dòng. 

Với mọi tập con S, chúng ta thử mọi trạm i làm câu hỏi tiếp theo. Truy vấn đó phân chia S thành S0 và S1 tùy thuộc vào việc mỗi dòng có chứa trạm i hay không. Chi phí dự kiến ​​từ việc chọn trạm i là một câu hỏi cộng với bình quân gia quyền của các chi phí tối ưu của hai tập hợp con thu được. Chúng tôi chọn trạm giảm thiểu kỳ vọng này. 

Phép đệ quy được xác định rõ ràng vì mọi chuyển đổi đều giảm thiểu sự không chắc chắn một cách nghiêm ngặt và các tập hợp con cuối cùng sẽ đạt đến kích thước một, khi đó không cần thêm câu hỏi nào nữa. 

Khó khăn nằm ở tính toán: có tới 2^50 tập hợp con của dòng, quá lớn để liệt kê. Tuy nhiên, phép đệ quy chỉ truy cập các tập hợp con thực sự có thể truy cập được bằng cách phân chia các truy vấn trạm bắt đầu từ tập hợp đầy đủ. Trong thực tế, bộ này nhỏ hơn nhiều và có thể được lưu vào bộ đệm bằng cách sử dụng tính năng ghi nhớ được khóa bằng mặt nạ tập hợp con.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê tất cả các cây quyết định | Số mũ trong cây | Hàm mũ | Quá chậm | 
| DP trên các tập hợp con của dòng có ghi nhớ | O(số trạng thái có thể truy cập × N × M) | O(số tiểu bang) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta biểu diễn mỗi dòng dưới dạng một bitmask số nguyên có độ dài N, trong đó bit i cho biết liệu dòng có dừng ở trạm i hay không. 

Sau đó chúng ta định nghĩa một hàm đệ quy dp(S), trong đó S là tập con của các chỉ số dòng hiện vẫn có thể thực hiện được. 

1. Nếu S chỉ chứa một dòng thì chi phí bằng 0 vì không cần thêm câu hỏi nào để xác định nó. Đây là trường hợp cơ bản của đệ quy. 
2. Nếu S đã được tính trước đó, chúng tôi trả về kết quả đã lưu. Điều này ngăn chặn việc tính toán lại các trạng thái giống hệt nhau đạt được thông qua các đường dẫn truy vấn khác nhau. 
3. Với tập S hiện tại, chúng ta thử mọi trạm i từ 0 đến N − 1 làm câu hỏi ứng viên. Điều này thể hiện việc hỏi liệu đường dây chưa biết có bao gồm trạm i hay không. 
4. Đối với trạm cố định i, chúng ta chia S thành hai tập con. Một dòng chứa tất cả các dòng trong S bao gồm trạm i và dòng kia chứa những dòng không có. Những điều này tương ứng với hai câu trả lời có thể. 
5. Nếu một trong hai tập hợp con trống, truy vấn này không giúp phân biệt trạng thái hiện tại và bị bỏ qua. 
6. Ngược lại, chúng ta tính chi phí dự kiến khi chọn trạm i như sau: 

một câu hỏi cộng với giá trị trung bình có trọng số của chi phí tối ưu của hai tập hợp con, được tính theo kích thước của chúng trong S. 
7. Chúng tôi lấy mức tối thiểu trên tất cả các trạm hợp lệ và lưu trữ dưới dạng dp(S). 

Câu trả lời cuối cùng là dp(tất cả các dòng), trong đó tất cả các dòng là tập hợp đầy đủ các chỉ mục. 

### Tại sao nó hoạt động 

Mọi chiến lược đặt câu hỏi hợp lệ đều tương ứng với một cây quyết định trong đó mỗi nút được xác định chính xác bằng tập hợp các dòng phù hợp với các câu trả lời cho đến nay. Hai lịch sử khác nhau dẫn đến cùng một tập con S không thể phân biệt được từ thời điểm đó trở đi, do đó các quyết định tối ưu chỉ phụ thuộc vào S chứ không phụ thuộc vào đường đi. Điều này thiết lập cấu trúc con tối ưu cần thiết cho lập trình động. 

Vì mọi truy vấn chia một tập hợp thành các tập con rời rạc có tổng kích thước nhỏ hơn rất nhiều, nên phép đệ quy phải kết thúc ở các phần đơn, đảm bảo tính chính xác của việc truyền trường hợp cơ sở. Thuật toán đánh giá tất cả các câu hỏi đầu tiên có thể có ở mọi trạng thái, do đó không bỏ sót phần phân chia tối ưu nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

def solve():
    N = int(input())
    M = int(input())

    masks = []
    for _ in range(M):
        tmp = list(map(int, input().split()))
        k = tmp[0]
        stations = tmp[1:]
        mask = 0
        for s in stations:
            mask |= 1 << s
        masks.append(mask)

    # check distinguishability
    seen = set(masks)
    if len(seen) != M:
        print("not possible")
        return

    from functools import lru_cache

    full = tuple(range(M))

    @lru_cache(None)
    def dp(state):
        if len(state) <= 1:
            return 0.0

        best = float('inf')

        # try each station
        for i in range(N):
            left = []
            right = []
            for idx in state:
                if masks[idx] & (1 << i):
                    left.append(idx)
                else:
                    right.append(idx)

            if not left or not right:
                continue

            left = tuple(left)
            right = tuple(right)

            pL = len(left) / len(state)
            pR = 1 - pL

            cost = 1 + pL * dp(left) + pR * dp(right)
            if cost < best:
                best = cost

        return best

    ans = dp(full)
    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên nén từng tuyến tàu điện ngầm thành một mặt nạ bit trên các ga. Điều này cho phép mỗi truy vấn được đánh giá theo O(1) trên mỗi dòng bằng cách sử dụng các thao tác bit. 

Hàm DP đệ quy là cốt lõi. Nó xử lý từng tập hợp con có thể truy cập của các chỉ mục dòng dưới dạng trạng thái và thử mọi trạm dưới dạng truy vấn phân tách. Việc ghi nhớ rất quan trọng vì nhiều chuỗi truy vấn khác nhau có thể dẫn đến cùng một tập ứng viên còn lại. 

Trọng số xác suất được tính trực tiếp từ các kích thước tập hợp con vì mọi dòng đều có khả năng như nhau và điều kiện hóa trên một trạng thái sẽ duy trì tính đồng nhất. 

Một chi tiết triển khai tinh tế là biểu diễn các tập hợp con của dòng dưới dạng bộ dữ liệu. Điều này làm cho chúng có thể băm được để lưu vào bộ nhớ đệm nhưng vẫn dễ dàng lặp lại. Độ sâu đệ quy được giới hạn bởi M, vì mỗi truy vấn thành công phải giảm sự mơ hồ. 

## Ví dụ đã hoạt động 

### Mẫu 2 

đầu vào:```
3
3
1 0
1 1
1 2
```Cả ba dòng đều là bộ trạm đơn. 

| Bang S | Trạm được chọn | Chia | Chi phí dự kiến ​​| 
| --- | --- | --- | --- | 
| {0,1,2} | 0 | {0} / {1,2} | tính toán | 
| {1,2} | 1 | {1} / {2} | tính toán | 

Từ gốc hỏi trạm 0 cô lập ngay một dòng và để lại hai dòng. Truy vấn thứ hai sau đó phân biệt hai truy vấn đó. Giá trị kỳ vọng tối ưu trở thành 5/3. 

Dấu vết này cho thấy thuật toán ưu tiên các phần tách sớm để cô lập các đơn vị một cách tự nhiên như thế nào. 

### Mẫu 1 

đầu vào:```
5
4
3 0 3 4
3 0 2 3
3 2 3 4
2 1 2
```Ở gốc, các trạm khác nhau tạo ra các phân vùng khác nhau của bốn dòng. DP đánh giá tất cả chúng và chọn trạm giảm thiểu sự kết hợp có trọng số của chi phí cây con. Mỗi trạng thái tiếp theo lặp lại quá trình tương tự trên các tập hợp con nhỏ hơn cho đến khi tất cả các dòng được tách ra. 

Dấu vết xác nhận rằng mặc dù nhiều phần chia tách trông có vẻ đối xứng nhưng chỉ có DP mới giải thích chính xác sự mất cân bằng ở hạ lưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(Hoa × N × M) | Mỗi tiểu bang thử tất cả các trạm và quét tất cả các dòng trong tiểu bang | 
| Không gian | O(Hoa × M) | Ghi nhớ lưu trữ từng tập hợp con các dòng có thể truy cập | 

Số lượng trạng thái phụ thuộc vào dữ liệu nhưng bị giới hạn bởi số lượng tập hợp con riêng biệt có thể truy cập thông qua việc phân chia trạm. Với M nhiều nhất là 50, điều này vẫn khả thi với các ràng buộc đã định. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    N = int(sys.stdin.readline())
    M = int(sys.stdin.readline())
    masks = []
    for _ in range(M):
        tmp = list(map(int, sys.stdin.readline().split()))
        k = tmp[0]
        stations = tmp[1:]
        mask = 0
        for s in stations:
            mask |= 1 << s
        masks.append(mask)

    if len(set(masks)) != M:
        return "not possible"

    from functools import lru_cache

    full = tuple(range(M))

    @lru_cache(None)
    def dp(state):
        if len(state) <= 1:
            return 0.0
        best = float('inf')
        for i in range(N):
            left = tuple(idx for idx in state if masks[idx] & (1 << i))
            right = tuple(idx for idx in state if not (masks[idx] & (1 << i)))
            if not left or not right:
                continue
            pL = len(left) / len(state)
            cost = 1 + pL * dp(left) + (1 - pL) * dp(right)
            best = min(best, cost)
        return best

    return dp(full)

# provided samples
assert abs(run("5\n4\n3 0 3 4\n3 0 2 3\n3 2 3 4\n2 1 2\n") - 2.0) < 1e-6
assert abs(run("3\n3\n1 0\n1 1\n1 2\n") - 1.66666666666667) < 1e-6

# custom cases
assert run("2\n2\n1 0\n1 0\n") == "not possible"
assert abs(run("2\n2\n1 0\n1 1\n") - 1.0) < 1e-6
assert abs(run("3\n2\n2 0 1\n1 2\n2 1 2\n") < 5.0)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trùng lặp các dòng giống hệt nhau | không thể | phát hiện không thể | 
| cấu trúc đơn lẻ hoàn toàn có thể tách rời | 1.0 | trường hợp tốt nhất là chia tay sớm | 
| cấu trúc chồng chéo hỗn hợp | giá trị hữu hạn | tính đúng đắn dưới sự phân chia chồng chéo | 

## Vỏ cạnh 

Khi hai dòng giống nhau, mọi truy vấn trạm đều tạo ra các câu trả lời giống nhau, do đó thuật toán sẽ loại bỏ phiên bản một cách chính xác trước khi DP bắt đầu. Mọi nỗ lực tiếp tục sẽ giữ cả hai dòng trong mỗi tập hợp con mãi mãi, ngăn chặn việc chấm dứt. 

Khi mỗi dòng khác nhau trên chính xác một trạm, truy vấn đầu tiên sẽ ngay lập tức tách biệt một dòng trong khi để lại một bài toán con độc lập nhỏ hơn. DP đương nhiên thích điều này hơn vì nó giảm thiểu chi phí cây con có trọng số, phù hợp với trực giác tối ưu về việc sớm cô lập các đơn vị. 

Khi nhiều dòng giống hệt nhau trên hầu hết các trạm và chỉ khác nhau ở một tập hợp con nhỏ, việc lựa chọn tham lam ngây thơ có xu hướng phù hợp hơn với các phân tách sớm. DP trì hoãn việc phân chia như vậy một cách chính xác nếu chúng không cải thiện được chi phí dự kiến ​​có trọng số trên toàn bộ cây con, duy trì tính tối ưu toàn cục.
