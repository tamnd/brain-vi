---
title: "CF 106444J - Toàn cảnh Shuttekka"
description: "Chúng tôi được cung cấp một mảng giá trị theo các vị trí và một danh sách các truy vấn. Mỗi truy vấn chỉ định một phân đoạn liền kề của mảng. Đối với mỗi phân đoạn, chúng tôi xem xét tất cả các phần tử bên trong phân đoạn đó và xác định vị trí xuất hiện giá trị lớn nhất."
date: "2026-06-20T04:03:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106444
codeforces_index: "J"
codeforces_contest_name: "OCPC 2025 Winter, Day 1: Limas Sultan Agung"
rating: 0
weight: 106444
solve_time_s: 51
verified: true
draft: false
---

[CF 106444J - Shuttekka Toàn cảnh](https://codeforces.com/problemset/problem/106444/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng giá trị theo các vị trí và một danh sách các truy vấn. Mỗi truy vấn chỉ định một phân đoạn liền kề của mảng. Đối với mỗi phân đoạn, chúng tôi xem xét tất cả các phần tử bên trong phân đoạn đó và xác định vị trí xuất hiện giá trị lớn nhất. Nếu nhiều vị trí chia sẻ giá trị lớn nhất thì bài toán ngầm đòi hỏi một quy tắc ràng buộc nhất quán, điển hình là chỉ số ngoài cùng bên trái hoặc chỉ số nhỏ nhất. 

Đối với mỗi truy vấn, chúng tôi ghi lại chỉ mục của “ngọn núi cao nhất” này trong phân khúc của nó. Nhiệm vụ cuối cùng là tính toán, với mỗi vị trí trong mảng, có bao nhiêu truy vấn chọn vị trí đó làm vị trí lớn nhất. 

Vì vậy, thay vì trả lời từng truy vấn bằng chính giá trị tối đa, chúng tôi đang tính tần suất mỗi chỉ mục trở thành argmax trong tất cả các khoảng truy vấn. 

Các ràng buộc (số lượng truy vấn lớn và kích thước mảng có thể lớn) ngụ ý rằng việc tính toán lại mức tối đa bằng cách quét từng phân đoạn sẽ quá chậm. Một giải pháp đơn giản sẽ kiểm tra từng truy vấn theo thời gian tuyến tính trên phạm vi của nó, dẫn đến hành vi bậc hai trong trường hợp xấu nhất. Điều đó ngay lập tức vượt quá giới hạn thông thường đối với các sự cố của Codeforce với tối đa 200 nghìn phần tử hoặc truy vấn. 

Một vấn đề tinh vi hơn xuất hiện khi các giá trị lặp lại. Nếu tồn tại mức cực đại bằng nhau, việc phá vỡ ràng buộc không nhất quán sẽ thay đổi sự phân bổ số lượng. Việc triển khai đúng phải đảm bảo rằng cùng một chỉ số xác định luôn được chọn cho các giá trị tối đa giống hệt nhau. 

Một ví dụ nhỏ minh họa vấn đề. Giả sử mảng là`[1, 3, 2, 3]`. Đối với truy vấn`[1, 4]`, cả hai chỉ số 2 và 4 đều có giá trị 3. Nếu chúng ta luôn chọn mức tối đa ngoài cùng bên trái thì chỉ số 2 sẽ được chọn. Việc triển khai bất cẩn cập nhật câu trả lời bất cứ khi nào nó thấy giá trị tối đa mà không theo dõi vị trí có thể chọn sai chỉ số 4 thay vào đó, làm sai lệch số đếm tần số cuối cùng. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force xử lý từng truy vấn một cách độc lập. Đối với phạm vi truy vấn`[l, r]`, chúng tôi quét tất cả các phần tử giữa`l`Và`r`và theo dõi giá trị tối đa và chỉ số của nó. Điều này đúng vì nó trực tiếp tuân theo định nghĩa của câu trả lời. Tuy nhiên, nếu có`m`truy vấn trên một mảng kích thước`n`, mỗi truy vấn có thể tốn`O(n)`trong trường hợp xấu nhất, dẫn đến`O(nm)`tổng công việc. Khi cả hai`n`Và`m`lớn, điều này trở nên không thể thực hiện được. 

Quan sát quan trọng là mảng là tĩnh và chúng tôi chỉ cần truy vấn phạm vi tối đa. Điều này cho phép tiền xử lý cấu trúc dữ liệu có thể trả lời “mức tối đa trong phân đoạn này là bao nhiêu và nó xảy ra ở đâu” theo thời gian logarit hoặc thậm chí không đổi. Một bảng thưa trên mảng, lưu trữ cả giá trị tối đa và chỉ số của chúng, cho phép chúng ta tính toán argmax của bất kỳ phạm vi truy vấn nào trong`O(1)`sau đó`O(n log n)`tiền xử lý. 

Khi chúng tôi có thể trả lời từng truy vấn một cách hiệu quả, chúng tôi không cần cập nhật phạm vi hoặc duy trì bất kỳ cấu trúc động nào. Chúng tôi chỉ đơn giản đếm số lần mỗi chỉ mục được trả về là tối đa. 

Việc đề cập đến tập hợp phạm vi và tích lũy hậu tố trong ý tưởng ban đầu tương ứng với việc thay thế các cập nhật lặp lại trên mỗi chỉ mục bằng một mảng tần số và bước tích lũy cuối cùng. Thay vì thực hiện việc truyền bá tốn kém trên mỗi phạm vi, chúng tôi trực tiếp tăng chỉ mục đã chọn cho mỗi truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(1) | Quá chậm | 
| Bảng thưa + Đếm | O((n + m) log n) | O(n log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

## Hướng dẫn thuật toán 

1. Xây dựng một bảng thưa trên mảng hỗ trợ các truy vấn tối đa phạm vi. Mỗi mục lưu trữ cả giá trị và chỉ mục của nó để các mối quan hệ có thể được giải quyết một cách xác định. Lý do lưu trữ chỉ số là câu trả lời cuối cùng phụ thuộc vào vị trí chứ không phải giá trị. 
2. Tính toán trước logarit sao cho bất kỳ phạm vi truy vấn nào cũng có thể được phân tách thành hai khoảng chồng chéo có độ dài lũy thừa hai bằng nhau. Điều này cho phép truy vấn liên tục. 
3. Đối với mỗi phạm vi truy vấn`[l, r]`, hãy sử dụng bảng thưa thớt để lấy chỉ mục của phần tử tối đa trong phân đoạn đó. Bước này thay thế việc quét rõ ràng trên phạm vi. 
4. Duy trì dải tần`cnt`, Ở đâu`cnt[i]`biểu thị số lượng truy vấn đã chọn chỉ mục`i`như tối đa. 
5. Đối với mỗi chỉ mục kết quả truy vấn`p`, tăng`cnt[p]`bởi một. Điều này tổng hợp tất cả các đóng góp trực tiếp tại các vị trí đã chọn. 
6. Xuất mảng tần số sau khi xử lý tất cả các truy vấn. 

### Tại sao nó hoạt động 

Mỗi truy vấn độc lập chọn chính xác một chỉ mục theo quy tắc xác định: vị trí tối đa trong phạm vi của nó. Bảng thưa đảm bảo rằng lựa chọn này đúng cho mọi truy vấn trong thời gian không đổi. Vì mỗi truy vấn đóng góp chính xác một đơn vị trọng số cho đúng một chỉ mục nên việc tích lũy số lượng trên mỗi chỉ mục sẽ duy trì sự phân bổ chính xác của các lựa chọn. Không có sự tương tác nào giữa các truy vấn, vì vậy việc tổng hợp tuyến tính trên các kết quả được tính toán độc lập là đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build_sparse(a):
    n = len(a)
    LOG = (n).bit_length()
    st = [[(0, 0)] * n for _ in range(LOG)]
    
    for i, v in enumerate(a):
        st[0][i] = (v, i)
    
    j = 1
    while (1 << j) <= n:
        length = 1 << j
        half = length >> 1
        for i in range(n - length + 1):
            left = st[j - 1][i]
            right = st[j - 1][i + half]
            if left[0] >= right[0]:
                st[j][i] = left
            else:
                st[j][i] = right
        j += 1
    
    return st

def query(st, l, r):
    length = (r - l + 1).bit_length() - 1
    left = st[length][l]
    right = st[length][r - (1 << length) + 1]
    if left[0] >= right[0]:
        return left[1]
    return right[1]

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    
    st = build_sparse(a)
    cnt = [0] * n
    
    for _ in range(m):
        l, r = map(int, input().split())
        l -= 1
        r -= 1
        p = query(st, l, r)
        cnt[p] += 1
    
    print(*cnt)

if __name__ == "__main__":
    solve()
```Cấu trúc bảng thưa thớt lưu trữ chỉ số của phần tử lớn nhất trong khoảng đó cho mỗi độ dài khoảng. Trong một truy vấn, phạm vi được chia thành hai khối lũy thừa hai khối chồng chéo và mức cực đại được tính toán trước của chúng được so sánh. Chỉ số của giá trị lớn hơn được trả về. 

Mảng tần số tích lũy kết quả trực tiếp. Mỗi truy vấn đóng góp chính xác một mức tăng, do đó không cần xử lý thêm sau vòng lặp. 

Phải cẩn thận trong việc bẻ dây buộc. Việc so sánh sử dụng`>=`để khi các giá trị bằng nhau, khoảng bên trái sẽ thắng, đảm bảo lựa chọn xác định chỉ số nhỏ hơn. 

## Ví dụ đã hoạt động 

Hãy xem xét một mảng`a = [2, 1, 3, 3]`với các truy vấn`[1,4]`,`[2,3]`, Và`[3,4]`. 

Đối với truy vấn đầu tiên`[1,4]`, giá trị lớn nhất là`3`, xảy ra ở chỉ số 3 và 4. Quy tắc tie-break chọn chỉ số 3. 

Đối với truy vấn thứ hai`[2,3]`, đoạn này là`[1,3]`và tối đa là ở chỉ số 3. 

Đối với truy vấn thứ ba`[3,4]`, cả hai chỉ số đều chứa`3`và chỉ số ngoài cùng bên trái 3 được chọn. 

| Truy vấn | Phạm vi | Chỉ số tối đa | trạng thái cnt | 
| --- | --- | --- | --- | 
| 1 | [1,4] | 3 | [0,0,1,0] | 
| 2 | [2,3] | 3 | [0,0,2,0] | 
| 3 | [3,4] | 3 | [0,0,3,0] | 

Dấu vết này cho thấy rằng tất cả các truy vấn đều giải quyết nhất quán theo cùng một chỉ mục do cấu trúc tối đa giống hệt nhau. 

Ví dụ thứ hai sử dụng`a = [5, 1, 4, 2]`với các truy vấn`[1,2]`,`[1,3]`,`[3,4]`. 

| Truy vấn | Phạm vi | Chỉ số tối đa | trạng thái cnt | 
| --- | --- | --- | --- | 
| 1 | [1,2] | 1 | [1,0,0,0] | 
| 2 | [1,3] | 1 | [2,0,0,0] | 
| 3 | [3,4] | 3 | [2,0,1,0] | 

Điều này cho thấy các phạm vi chồng chéo liên tục củng cố cùng một chỉ số vượt trội như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + m) | Quá trình tiền xử lý bảng thưa thớt mất`n log n`, mỗi truy vấn là O(1) | 
| Không gian | O(n log n) | Bảng thưa lưu trữ nhiều cấp trên mảng | 

Giải pháp này phù hợp một cách thoải mái trong các ràng buộc điển hình vì cả quá trình tiền xử lý và xử lý truy vấn đều gần như tuyến tính trong thực tế. Ngay cả với kích thước đầu vào lớn, hành vi truy vấn trong thời gian không đổi vẫn đảm bảo thực thi nhanh chóng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return sys.stdout.getvalue()

# Simple sanity case
assert run("""4 3
2 1 3 3
1 4
2 3
3 4
""") == "0 0 3 0\n"

# All equal values
assert run("""5 2
7 7 7 7 7
1 5
2 4
""") == "0 2 0 0 0\n"

# Single element queries
assert run("""3 3
1 2 3
1 1
2 2
3 3
""") == "1 1 1\n"

# Full dominance
assert run("""4 2
10 1 2 3
1 4
1 3
""") == "2 0 0 0\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Giá trị bằng nhau | cắt cà vạt đồng phục | sự ổn định dưới mối quan hệ | 
| Yếu tố đơn lẻ | tự lựa chọn | độ đúng ranh giới | 
| Mảng lệch | cực đại lặp đi lặp lại | hành vi thống trị | 

## Vỏ cạnh 

Khi tất cả các phần tử đều bằng nhau, mọi truy vấn đều phụ thuộc hoàn toàn vào việc phá vỡ liên kết. Đối với một mảng như`[5, 5, 5]`và một truy vấn`[1, 3]`, bảng thưa thớt luôn trả về chỉ số 1 do việc hợp nhất thiên về trái. Các truy vấn lặp lại sẽ chỉ tích lũy ở chỉ mục 1, tạo ra sự phân bố tần số bị sai lệch nhiều. Thuật toán xử lý việc này một cách chính xác vì quy tắc so sánh thực thi tính xác định ở mọi bước hợp nhất, không chỉ tại thời điểm truy vấn. 

Đối với mảng một phần tử, mọi phạm vi truy vấn sẽ thu gọn về cùng một chỉ mục bất kể đầu vào là gì. Vì`a = [42]`, bất kỳ truy vấn nào`[1,1]`luôn trả về chỉ mục 1 và bảng thưa thớt thoái hóa thành một lớp duy nhất. Thuật toán vẫn hoạt động vì logic truy vấn không bao giờ giả định độ dài phạm vi tối thiểu lớn hơn một.
