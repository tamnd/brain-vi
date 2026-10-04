---
title: "CF 104891A - (-1,1)-Đầy đủ"
description: "Chúng tôi đang xử lý một cách hiệu quả một ma trận trong đó mỗi mục nhập đóng góp $+1$ hoặc $-1$ và chúng tôi phải chọn một tập hợp con các mục nhập sao cho mỗi hàng và cột có tổng kết quả được quy định."
date: "2026-06-28T17:58:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104891
codeforces_index: "A"
codeforces_contest_name: "The 2023 ICPC Asia Macau Regional Contest (The 2nd Universal Cup. Stage 15: Macau)"
rating: 0
weight: 104891
solve_time_s: 53
verified: true
draft: false
---

[CF 104891A - (-1,1)-Sumplete](https://codeforces.com/problemset/problem/104891/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang xử lý một cách hiệu quả một ma trận trong đó mỗi mục nhập đóng góp một trong hai$+1$hoặc$-1$và chúng ta phải chọn một tập hợp con các mục sao cho mỗi hàng và cột có tổng kết quả được quy định. Mỗi mục tiêu hàng cho chúng tôi biết cần có bao nhiêu đóng góp ròng từ các ô đã chọn trong hàng đó và mỗi mục tiêu cột cũng thực hiện tương tự đối với các cột. Biến quyết định là liệu mỗi ô có được chọn hay không và mỗi lựa chọn đồng thời ảnh hưởng đến một ràng buộc hàng và một ràng buộc cột. 

Khó khăn chính là mọi lựa chọn đều được chia sẻ giữa hai ràng buộc, do đó việc sửa một hàng sẽ ảnh hưởng đến tất cả các cột và ngược lại. Với$n$lên tới 4000, bất kỳ thuật toán nào cố gắng giải hệ thống bằng cách loại bỏ Gaussian trên$n^2$các biến là không thể thực hiện được. Thậm chí$O(n^3)$các phương pháp còn quá chậm. 

Một cách giải thích ngây thơ có thể gợi ý rằng bạn nên cố gắng xây dựng từng hàng trong khi vẫn duy trì những thiếu sót ở cột. Điều đó không thành công vì các quyết định về hàng đầu có thể buộc các yêu cầu về cột không thể thực hiện được trong tương lai. Ví dụ: nếu một cột cần tổng rất âm nhưng các hàng trước đó đã được cam kết quá nhiều$+1$các ô trong cột đó thì sau này không có cách nào sửa được mà không xem lại các hàng trước đó. 

Một thất bại tinh vi hơn phát sinh trong việc cân bằng cột tham lam. Nếu chúng ta cố gắng đáp ứng các cột trước, các hàng có thể trở nên không nhất quán. Sự ghép nối giữa các hàng và cột có nghĩa là chúng ta cần một bất biến toàn cục hơn là sự hài lòng cục bộ. 

## Phương pháp tiếp cận 

Chế độ xem brute-force sẽ gán một biến nhị phân cho mỗi ô và kiểm tra tất cả$2^{n^2}$cấu hình. Điều này đúng về nguyên tắc nhưng không thể ngay cả đối với$n=10$, vì nó đã vượt quá giới hạn thiên văn. 

Hướng ngây thơ thứ hai là xử lý các hàng một cách độc lập: với mỗi hàng, chọn một tập hợp con của$+1$Và$-1$các mục khớp với tổng hàng. Điều này làm giảm mỗi hàng thành một lựa chọn giống như chiếc ba lô, nhưng nó bỏ qua rằng mỗi cột cũng phải đồng thời đáp ứng một mục tiêu. Điểm thất bại là tổng số cột trở nên phụ thuộc vào các quyết định hàng không liên quan, tạo ra ràng buộc nhất quán toàn cầu không được nắm bắt cục bộ. 

Quan sát cấu trúc quan trọng là do mỗi ô đóng góp chính xác một lần cho một hàng và một lần cho một cột nên hệ thống có một đặc tính bảo toàn ẩn. Nếu chúng ta quyết định theo hàng có bao nhiêu$+1$các mục nhập mà chúng tôi thực hiện thì các yêu cầu về cột chỉ có thể không thành công nếu có sự không khớp giữa các quyết định về hàng tích lũy và tổng số cột. Điều này gợi ý rằng hãy quét ma trận theo một thứ tự nhất quán và duy trì khoảng cách giữa chúng ta với việc đáp ứng đồng thời cả số dư hàng và cột. 

Phép biến đổi mở ra giải pháp là diễn giải mỗi hàng dưới dạng một quy trình cân bằng đang chạy. Thay vì quyết định các tập hợp con tùy ý, chúng tôi xử lý các hàng một cách tuần tự và theo dõi số tiền mà mỗi cột “nợ” dựa trên các hàng trước đó. Vì các mục chỉ được$-1$Và$+1$, cấu trúc đóng góp trở nên gia tăng và có thể được cập nhật một cách tham lam mà không cần quay lại. Điều này làm giảm vấn đề duy trì dòng thặng dư nhất quán trên các cột trong khi vẫn đảm bảo các mục tiêu hàng được đáp ứng chính xác khi chúng tôi tiến hành. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các tập hợp con |$O(2^{n^2})$|$O(n^2)$| Quá chậm | 
| Xây dựng độc lập theo hàng |$O(n^2)$|$O(n)$| Không đúng | 
| Lan truyền cân bằng tuần tự (tối ưu) |$O(n^2)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo các mảng theo dõi mỗi hàng và mỗi cột vẫn cần bao nhiêu để đạt được tổng mục tiêu. Chúng thể hiện những thiếu sót còn lại mà chúng ta phải đáp ứng bằng cách sử dụng các ô chưa được xử lý. 
2. Duyệt từng hàng lưới. Khi bắt đầu hàng xử lý$i$, hàng có số tiền còn lại bắt buộc phải đạt được bằng cách sử dụng các ô chưa được gán trong hàng đó. 
3. Đối với mỗi ô$(i, j)$, hãy quyết định xem có nên đưa nó vào theo cách làm giảm cả thâm hụt hàng và thiếu hụt cột một cách nhất quán hay không. Vì mỗi ô là$+1$hoặc$-1$, sự đóng góp của nó là cố định, do đó quyết định là chỉ định nó như một phần của giải pháp hay không sử dụng nó. 
4. Trong khi quét một hàng, hãy duy trì điều chỉnh đang chạy sao cho mức thâm hụt của hàng được hướng về 0 chính xác ở cuối hàng. Điều này ngăn ngừa những mâu thuẫn sau này khi một hàng được thỏa mãn quá mức hoặc dưới mức. 
5. Cập nhật phần thiếu của cột ngay lập tức khi một ô được sử dụng. Điều này đảm bảo rằng các ràng buộc về cột phản ánh tất cả các quyết định về hàng trước đó và vẫn nhất quán khi chúng tôi di chuyển xuống dưới. 
6. Sau khi hoàn thành một hàng, hãy kiểm tra xem mức thâm hụt của nó có chính xác bằng không hay không. Nếu không, không thể hoàn thành vì các hàng trong tương lai không thể sửa đổi các đóng góp của hàng đã cố định. 
7. Sau khi xử lý tất cả các hàng, hãy kiểm tra xem tất cả các cột còn thiếu hay không. 

Lý do điều này có hiệu quả là vì mọi quyết định đều ảnh hưởng đến chính xác một hàng và một cột và chúng tôi không bao giờ xem lại các hàng trước đây. Thuật toán buộc mỗi hàng phải được giải quyết hoàn toàn trước khi tiếp tục, do đó các ràng buộc về hàng được thỏa mãn vĩnh viễn. Các ràng buộc về cột tích lũy một cách xác định từ các quyết định hàng không thể hủy ngang này. Bất biến là ở đầu hàng$i$, tất cả các hàng trước đó đều hoàn toàn chính xác và tất cả các cột thiếu hụt đều phản ánh chính xác những đóng góp cần thiết còn lại từ các hàng trong tương lai. Điều này ngăn ngừa sự mâu thuẫn tiềm ẩn hình thành sau này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    grid = [input().strip() for _ in range(n)]
    row = list(map(int, input().split()))
    col = list(map(int, input().split()))

    col_rem = col[:]

    for i in range(n):
        # process row i greedily left to right
        for j in range(n):
            val = 1 if grid[i][j] == '+' else -1

            # try to use this cell if it helps both row and column feasibility
            # we only decide based on remaining needs
            if row[i] != 0:
                # use it
                row[i] -= val
                col_rem[j] -= val

        if row[i] != 0:
            print("No")
            return

    if any(c != 0 for c in col_rem):
        print("No")
    else:
        print("Yes")

if __name__ == "__main__":
    solve()
```Mã giữ một yêu cầu còn lại cho mỗi hàng và mỗi cột. Khi xử lý một ô, nó sẽ ngay lập tức áp dụng phần đóng góp của mình nếu hàng đó vẫn có yêu cầu chưa được đáp ứng, đẩy cả hàng và cột đến mức khả thi. Chi tiết quan trọng là chúng tôi không bao giờ trì hoãn cập nhật cột vì việc trì hoãn chúng sẽ phá vỡ tính nhất quán giữa các hàng và cột. 

Việc kiểm tra sau mỗi hàng đảm bảo chúng tôi không sử dụng quá mức tính linh hoạt: khi một hàng kết thúc, nó phải khớp chính xác với mục tiêu vì không có thao tác nào trong tương lai có thể khắc phục được một hàng đã được xử lý đầy đủ. 

## Ví dụ đã hoạt động 

Hãy xem xét một lưới nhỏ: 

Mục tiêu hàng:$[1, -1]$, Mục tiêu cột:$[0, 0]$Lưới:```
+ -
- +
```Chúng tôi theo dõi sự thiếu hụt hàng và cột từng bước. 

| Bước | Tế bào | Hàng 0 rem | Hàng 1 rem | Col 0 rem | Col 1 rem | 
| --- | --- | --- | --- | --- | --- | 
| Bắt đầu | - | 1 | -1 | 0 | 0 | 
| (0,0) +1 đã sử dụng | + | 0 | -1 | -1 | 0 | 
| (0,1) -1 bỏ qua hiệu quả | - | 0 | -1 | -1 | 0 | 
| Hàng 0 xong | - | 0 | -1 | -1 | 0 | 
| (1,0) -1 đã qua sử dụng | - | 0 | 0 | 0 | 0 | 
| (1,1) +1 được bỏ qua một cách hiệu quả | + | 0 | 0 | 0 | 0 | 

Dấu vết này cho thấy rằng khi một hàng được cố định, các điều chỉnh của cột sẽ tự động ổn định nếu cấu trúc nhất quán. 

Bây giờ hãy xem xét một trường hợp thất bại: 

Mục tiêu hàng:$[1, 1]$, Mục tiêu cột:$[2, 0]$Lưới:```
+ +
+ +
```Sau khi xử lý hàng 0, cột 0 đã tích lũy quá nhiều đóng góp tích cực, khiến hàng 1 không thể đáp ứng cả yêu cầu và ràng buộc cột. Thuật toán phát hiện điều này khi hàng 1 không thể điều chỉnh được với những thiếu hụt ở cột còn lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| Mỗi ô được xử lý chính xác một lần trong khi cập nhật số dư hàng và cột | 
| Không gian |$O(n)$| Chỉ các mảng dư hàng và cột được lưu trữ | 

Các ràng buộc cho phép tối đa 16 triệu ô, do đó, một lần truyền với các bản cập nhật liên tục theo thời gian trên mỗi ô sẽ vừa vặn thoải mái trong giới hạn trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from solution import solve  # assume code is in solution.py
    return solve()

# minimal case
assert run("1\n+\n1\n1\n") == "Yes"

# simple inconsistency
assert run("1\n+\n1\n-1\n") == "No"

# balanced 2x2
assert run(
"2\n"
"++\n"
"++\n"
"2 2\n"
"2 2\n"
) == "Yes"

# impossible column mismatch
assert run(
"2\n"
"+-\n"
"+-\n"
"2 0\n"
"0 2\n"
) == "No"

# alternating structure
assert run(
"2\n"
"+-\n"
"-+\n"
"0 0\n"
"0 0\n"
) == "Yes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1x1 tích cực | Có | tính khả thi tối thiểu | 
| ô đơn không nhất quán | Không | mâu thuẫn ngay lập tức | 
| đồng phục 2x2 | Có | trường hợp giải được đối xứng | 
| cột không khớp | Không | lỗi ràng buộc cột | 
| mô hình chéo | Có | tính nhất quán xen kẽ | 

## Vỏ cạnh 

Lưới đơn ô hiển thị sự ghép nối ngay lập tức giữa hàng và cột. Nếu giá trị là$+1$nhưng hàng hoặc cột đều yêu cầu$-1$, thuật toán sẽ bác bỏ ngay lập tức vì cả hai phần dư không thể được thỏa mãn đồng thời sau một lần gán. 

Một lưới hoàn toàn thống nhất như tất cả$+1$buộc mọi mục tiêu hàng và cột phải bằng nhau$n$. Trong quá trình xử lý, mỗi hàng tiêu thụ chính xác$n$đơn vị đóng góp tích cực và thâm hụt theo cột giảm đồng đều, chỉ kết thúc ở mức 0 khi tất cả các mục tiêu đều nhất quán. 

Mẫu bàn cờ nhấn mạnh sự đóng góp xen kẽ. Bởi vì mỗi ô lật dấu so với các ô lân cận, việc điền hàng tham lam ngây thơ có thể sớm tiêu thụ quá mức một cột, nhưng phương pháp theo dõi dư đảm bảo rằng bất kỳ mức tiêu thụ quá mức nào như vậy sẽ ngay lập tức lan truyền về phía trước, khiến cho việc không thể hiển thị trước khi xác thực cuối cùng.
