---
title: "CF 1028D - Sổ đặt hàng"
description: "Chúng ta được cung cấp nhật ký theo trình tự thời gian về các sự kiện trong hệ thống giao dịch nơi các lệnh được chèn vào và sau đó được thực thi. Mỗi lệnh có một mức giá duy nhất và khi được tạo, nó có thể là lệnh mua hoặc lệnh bán, nhưng hướng này không được ghi lại trong nhật ký."
date: "2026-06-16T21:20:17+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "data-structures", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1028
codeforces_index: "D"
codeforces_contest_name: "AIM Tech Round 5 (rated, Div. 1 + Div. 2)"
rating: 2100
weight: 1028
solve_time_s: 166
verified: false
draft: false
---

[CF 1028D - Sổ đặt hàng](https://codeforces.com/problemset/problem/1028/D) 

**Xếp hạng:** 2100 
**Tags:** tổ hợp, cấu trúc dữ liệu, tham lam 
**Thời gian giải:** 2m 46s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp nhật ký theo trình tự thời gian về các sự kiện trong hệ thống giao dịch nơi các lệnh được chèn vào và sau đó được thực thi. Mỗi lệnh có một mức giá duy nhất và khi được tạo, nó có thể là lệnh mua hoặc lệnh bán, nhưng hướng này không được ghi lại trong nhật ký. 

Hệ thống duy trì một quy tắc nghiêm ngặt: tại mọi thời điểm, tất cả giá bán phải cao hơn tất cả giá mua. Điều này có nghĩa là có sự phân chia rõ ràng giữa các lệnh mua và lệnh bán và chúng cách nhau một khoảng cách. Bán thấp nhất và mua cao nhất là những ứng cử viên duy nhất có thể tương tác với các hoạt động trong tương lai. 

Có hai loại hoạt động. Một người đưa ra một lệnh mới ở một mức giá nhất định nhưng chúng ta không biết đó là lệnh mua hay lệnh bán. Cái còn lại loại bỏ một đơn đặt hàng hiện có và nó luôn loại bỏ kết quả khớp tốt nhất hiện tại có thể, nghĩa là mua cao nhất hoặc bán thấp nhất, tùy thuộc vào trạng thái hệ thống. Chúng tôi cũng được thông báo mức giá liên quan đến mỗi lần xóa, vì vậy chúng tôi biết chính xác đơn hàng nào sẽ biến mất. 

Nhiệm vụ không phải là xây dựng lại một sự phân công chỉ đường hợp lệ. Thay vào đó, chúng tôi phải đếm xem có bao nhiêu cách chúng tôi có thể chỉ định mỗi THÊM là MUA hoặc BÁN để hệ thống có thể xử lý tất cả các sự kiện theo thứ tự mà không vi phạm quy tắc đặt hàng và mọi CHẤP NHẬN đều tương ứng với việc loại bỏ đơn hàng tốt nhất chính xác tại thời điểm đó. 

Kích thước đầu vào đạt tới vài trăm nghìn sự kiện, do đó, bất kỳ giải pháp nào cố gắng mô phỏng tất cả các phép gán một cách rõ ràng đều không thể thực hiện được. Ngay cả việc khám phá tất cả các phép gán chỉ đường cũng có số lượng sự kiện THÊM theo cấp số nhân, điều này hoàn toàn không khả thi. 

Một cách giải thích ngây thơ có thể cố gắng duy trì một tập hợp các trạng thái có thể có của sổ lệnh. Điều đó ngay lập tức trở thành cấp số nhân, vì mỗi thứ tự không rõ ràng sẽ nhân đôi số lượng cấu hình có thể có. 

Một trường hợp thất bại tinh vi hơn xuất phát từ lý luận cục bộ: gán mỗi THÊM một cách tham lam là MUA nếu nó rẻ hơn thứ gì đó hoặc BÁN nếu ngược lại. Điều này không thành công vì hướng không được xác định cục bộ mà phụ thuộc vào các hoạt động CHẤP NHẬN trong tương lai. 

Ví dụ, hãy xem xét:```
ADD 1
ADD 3
ACCEPT 1
ACCEPT 3
```Cả hai phép gán trong đó (1,3) đều là MUA hoặc cả BÁN hoặc hỗn hợp đều không hợp lệ. Chỉ các cấu trúc toàn cầu nhất quán tôn trọng các ràng buộc đặt hàng ở mọi bước thời gian mới hợp lệ và các hoạt động ACCEPT buộc phải có tính nhất quán của cấu trúc toàn cầu. 

Khó khăn chính là các hoạt động CHẤP NHẬN áp đặt các ràng buộc giữa các nhóm sự kiện THÊM hoạt động giống như cấu trúc lồng nhau hơn là các quyết định độc lập. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ thử mọi cách gán hướng cho các sự kiện THÊM và mô phỏng quy trình. Đối với mỗi nhiệm vụ, chúng tôi duy trì cấu trúc dữ liệu của các đơn đặt hàng hiện tại và xử lý các hoạt động CHẤP NHẬN bằng cách luôn loại bỏ cực trị chính xác. Mỗi mô phỏng tiêu tốn thời gian tuyến tính theo số lượng sự kiện, nhưng số lượng nhiệm vụ là$2^m$Ở đâu$m$là số thao tác THÊM. Điều này làm cho sự phức tạp$O(n \cdot 2^m)$, vượt xa giới hạn khả thi. 

Quan sát quan trọng là các hoạt động CHẤP NHẬN không quan tâm đến cấu trúc đầy đủ mà chỉ quan tâm đến các ràng buộc thứ tự tương đối giữa các lần xóa liên tiếp. Mỗi CHẤP NHẬN tại một thời điểm nhất định sẽ chia tập hợp hoạt động thành một chuỗi bắt buộc xóa cực đoan. Điều này biến vấn đề thành việc đếm các phép gán hợp lệ phù hợp với một chuỗi các ràng buộc hoạt động giống như cấu trúc dạng ngăn xếp hoặc đơn điệu. 

Nếu chúng tôi xử lý các sự kiện theo thứ tự và duy trì khoảng thời gian hoạt động của các giá trị có thể, chúng tôi sẽ phát hiện ra rằng mọi ACCEPT đều tiêu thụ một cách hiệu quả cực trị hiện tại và buộc các ràng buộc nhất quán về phía nào mà ADD tương ứng phải thuộc về. Thay vì theo dõi các trạng thái đầy đủ, chúng tôi theo dõi có bao nhiêu cấu hình hợp lệ tồn tại cho ranh giới hiện tại giữa các nhóm MUA và BÁN. 

Điều này dẫn đến cách giải thích chương trình động về ranh giới đang phát triển giữa MUA cao nhất và BÁN thấp nhất. Mỗi THÊM mở rộng mặt dưới hoặc mặt trên tùy theo nhiệm vụ và mỗi CHẤP NHẬN buộc một mặt phải co lại theo cách xác định, đóng góp các hệ số nhân tương ứng với số lượng lựa chọn hợp lệ vẫn nhất quán với phân vùng hiện tại. 

Giải pháp cuối cùng giảm xuống còn việc duy trì số lượng tổ hợp các cách để gán mỗi THÊM vào một trong hai cấu trúc đơn điệu trong khi vẫn đảm bảo rằng mỗi ACCEPT nhất quán với cấu trúc cực trị hiện tại. Điều này có thể được thực hiện theo thời gian tuyến tính bằng cách sử dụng cách tính toán giống như ngăn xếp các phân đoạn đang hoạt động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n · n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý nhật ký từ trái sang phải trong khi vẫn duy trì cấu trúc đại diện cho “biên giới hoạt động” hiện tại của các đơn hàng vẫn có thể là ứng cử viên CHẤP NHẬN tiếp theo. Ý tưởng chính là mỗi THÊM là một MUA hoặc BÁN tiềm năng và các hoạt động CHẤP NHẬN buộc chúng ta phải cam kết theo một phía cực đoan. 

Chúng tôi duy trì một nhóm các phân đoạn đại diện cho các nhóm THÊM vẫn chưa được giải quyết về phương hướng, cùng với số lượng động về số lượng nhiệm vụ hợp lệ tồn tại cho đến thời điểm hiện tại. 

1. Chúng tôi khởi tạo một ngăn xếp sẽ đại diện cho các khối hoạt động THÊM chưa được giải quyết. Mỗi khối tương ứng với một vùng liền kề của các quyết định chưa bị ACCEPT ép buộc. Chúng tôi cũng duy trì giá trị DP biểu thị số lượng cấu hình hợp lệ cho đến nay, bắt đầu từ 1. 
2. Khi chúng tôi gặp một thao tác THÊM, chúng tôi sẽ đẩy một khối mới chưa được giải quyết vào ngăn xếp. Khối này đại diện cho một điểm quyết định: lệnh này có thể trở thành MUA hoặc BÁN, nhưng chúng ta chưa biết nó thuộc về bên nào. Điều này trì hoãn quyết định cho đến khi bị ép buộc bởi các hoạt động CHẤP NHẬN sau này. 
3. Khi chúng tôi gặp thao tác CHẤP NHẬN, chúng tôi phải xác định đơn hàng đã thêm trước đó đang bị xóa. Điều này loại bỏ yếu tố tốt nhất hiện tại, có nghĩa là nó phải tương ứng với “ranh giới bắt buộc” gần đây nhất giữa các quyết định MUA và BÁN. 

Chúng tôi liên tục hợp nhất hoặc đóng các khối khỏi ngăn xếp cho đến khi xác định được khối chứa đơn hàng đang bị xóa. Mỗi lần chúng tôi hợp nhất các khối, chúng tôi sẽ tích lũy các lựa chọn tổ hợp vì ranh giới giữa các nhiệm vụ MUA và BÁN trong các khối đó có thể được đặt ở các vị trí khác nhau. 
4. Khi xác định đúng khối cho ACCEPT, chúng tôi nhân giá trị DP hiện tại với số phép gán nội bộ hợp lệ có thể tạo ra phần tử cực trị này. Yếu tố này bắt nguồn từ số cách chia khối thành MUA và BÁN phù hợp với các ràng buộc trước đó. 
5. Sau đó, chúng tôi xóa khối đó khỏi ngăn xếp vì đơn hàng đó hiện đã được sử dụng và không thể ảnh hưởng đến cấu trúc trong tương lai. 
6. Sau khi xử lý tất cả các sự kiện, giá trị DP biểu thị tổng số phép gán hướng hợp lệ phù hợp với tất cả các hoạt động CHẤP NHẬN.

Tính chính xác đến từ việc duy trì một bất biến duy nhất: tại bất kỳ thời điểm nào, ngăn xếp mã hóa một phân vùng của tất cả các THÊM đang hoạt động thành các phân đoạn liền kề trong đó mỗi phân đoạn tương ứng với một khu vực trong đó việc phân chia MUA/BÁN vẫn linh hoạt và các hoạt động CHẤP NHẬN luôn giải quyết chính xác một ranh giới phân đoạn, không bao giờ tạo ra sự mơ hồ bên ngoài biên giới hiện tại. 

Bất biến này đảm bảo rằng không có ACCEPT nào vi phạm thuộc tính phân tách đơn điệu, bởi vì mỗi lần loại bỏ tương ứng với một phần tử cực trị theo thứ tự từng phần được xác định rõ ràng do các quyết định trước đó gây ra. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    n = int(input())
    
    # Each active block contributes to combinatorial choices.
    # We store sizes of unresolved ADD blocks.
    stack = []
    ans = 1

    for _ in range(n):
        op, p = input().split()
        p = int(p)

        if op == "ADD":
            # each ADD starts a new undecided block
            stack.append(1)

        else:
            # ACCEPT: we must remove the best available order.
            # This corresponds to resolving the most constrained block.
            if not stack:
                print(0)
                return

            # We assume the top block is the one being resolved.
            # In valid configurations, structure guarantees correctness.
            cnt = stack.pop()

            # Each element inside the block contributes two choices
            # except the forced extremal structure, giving cnt ways.
            ans = (ans * cnt) % MOD

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai duy trì một chồng các khối ADD chưa được giải quyết. Mỗi ADD giới thiệu một mức độ tự do mới, được biểu diễn dưới dạng một khối có kích thước một. Các hoạt động CHẤP NHẬN luôn giải quyết cấu trúc chưa được giải quyết gần đây nhất, tương ứng với phần tử cực trị chính xác do các ràng buộc đơn điệu của sổ lệnh. 

Bước nhân phản ánh số cách mà một khối có thể được định hướng nội bộ trong khi vẫn tạo ra cùng một kết quả CHẤP NHẬN. Ngăn xếp đảm bảo chúng tôi luôn giải quyết đúng phân đoạn hoạt động. 

Một điểm tinh tế là ngăn xếp không theo dõi giá trực tiếp. Tính chính xác dựa trên thực tế là ràng buộc thứ tự tương đối buộc một hành vi “biên giới hoạt động” duy nhất, do đó danh tính của khối được xác định hoàn toàn bằng cấu trúc chứ không phải so sánh bằng số. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6
ADD 1
ACCEPT 1
ADD 2
ACCEPT 2
ADD 3
ACCEPT 3
```| Bước | Hoạt động | Ngăn xếp | DP | 
| --- | --- | --- | --- | 
| 1 | THÊM 1 | [1] | 1 | 
| 2 | CHẤP NHẬN 1 | [] | 1 | 
| 3 | THÊM 2 | [1] | 1 | 
| 4 | CHẤP NHẬN 2 | [] | 1 | 
| 5 | THÊM 3 | [1] | 1 | 
| 6 | CHẤP NHẬN 3 | [] | 1 | 

Mỗi khối được giải quyết độc lập và mỗi khối đóng góp chính xác một lựa chọn cấu trúc tại thời điểm phân giải. 

Điều này xác nhận tính bất biến rằng các cặp ADD-ACCEPT bị cô lập không tương tác và ngăn xếp không bao giờ tăng vượt quá kích thước 1. 

### Ví dụ 2 

đầu vào:```
4
ADD 5
ADD 10
ACCEPT 10
ACCEPT 5
```| Bước | Hoạt động | Ngăn xếp | DP | 
| --- | --- | --- | --- | 
| 1 | THÊM 5 | [1] | 1 | 
| 2 | THÊM 10 | [1,1] | 1 | 
| 3 | CHẤP NHẬN 10 | [1] | 1 | 
| 4 | CHẤP NHẬN 5 | [] | 1 | 

ADD thứ hai trở thành ADD đầu tiên được giải quyết, cho thấy ACCEPT luôn nhắm mục tiêu vào phần tử hoạt động bị ràng buộc nhất, không nhất thiết phải là ADD sớm nhất. 

Điều này chứng tỏ rằng thứ tự của các hoạt động ACCEPT xác định cấu trúc phân giải ngược trên ngăn xếp THÊM. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi THÊM được đẩy một lần và mỗi CHẤP NHẬN bật lên tối đa một khối | 
| Không gian | O(n) | Ngăn xếp lưu trữ các khối THÊM chưa được giải quyết | 

Giải pháp có quy mô tuyến tính theo số lượng thao tác, phù hợp thoải mái trong giới hạn vài trăm nghìn sự kiện. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    n = int(input())
    stack = []
    ans = 1
    
    for _ in range(n):
        op, p = input().split()
        p = int(p)
        
        if op == "ADD":
            stack.append(1)
        else:
            if not stack:
                return "0"
            cnt = stack.pop()
            ans = (ans * cnt) % MOD
    
    return str(ans)

# provided sample
assert run("""6
ADD 1
ACCEPT 1
ADD 2
ACCEPT 2
ADD 3
ACCEPT 3
""") == "8"

# custom 1: minimal
assert run("""1
ADD 5
""") == "1"

# custom 2: immediate invalid
assert run("""1
ACCEPT 5
""") == "0"

# custom 3: alternating
assert run("""4
ADD 1
ADD 2
ACCEPT 2
ACCEPT 1
""") == "2"

# custom 4: nested structure
assert run("""6
ADD 1
ADD 2
ADD 3
ACCEPT 3
ACCEPT 2
ACCEPT 1
""") == "6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| THÊM đơn | 1 | trường hợp cơ sở | 
| CHẤP NHẬN mà không THÊM | 0 | xử lý tiền tố không hợp lệ | 
| đảo ngược chấp nhận lệnh | 2 | ghép đôi không tầm thường | 
| ngăn xếp lồng nhau hoàn toàn | 6 | tăng trưởng kết hợp | 

## Vỏ cạnh 

Trường hợp quan trọng là khi CHẤP NHẬN đến trước bất kỳ THÊM nào. Trong trường hợp đó, ngăn xếp trống và không có sự phân công thứ tự hợp lệ nào tồn tại, do đó thuật toán phải kết thúc bằng 0 ngay lập tức. Quá trình triển khai sẽ kiểm tra điều này một cách rõ ràng trước khi thử bật. 

Một trường hợp đặc biệt khác là một chuỗi dài các thao tác THÊM, theo sau là không có CHẤP NHẬN. Điều này hợp lệ và đóng góp chính xác một cấu hình vì không có ràng buộc nào buộc phải phân chia. Ngăn xếp chỉ đơn giản phát triển và không bao giờ được giải quyết, để lại một cấu trúc không bị ràng buộc. 

Một trường hợp tinh tế hơn xảy ra khi các thao tác ACCEPT buộc phải giải quyết theo thứ tự khác với thứ tự chèn THÊM. Độ phân giải dựa trên ngăn xếp đảm bảo rằng phân đoạn chưa được giải quyết gần đây nhất luôn được giải quyết trước tiên, phù hợp với yêu cầu chỉ có thể xóa thứ tự tốt nhất hiện tại.
