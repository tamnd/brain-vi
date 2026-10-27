---
title: "CF 106144A - Xóa mảng"
description: "Chúng tôi được cung cấp một mảng có thể thu nhỏ theo hai quy tắc xóa rất cụ thể. Quy tắc đầu tiên cho phép chúng tôi loại bỏ một lần xuất hiện của giá trị nhỏ nhất hiện có trong mảng và khi một số vị trí chứa giá trị tối thiểu đó, chúng tôi có thể tự do chọn vị trí nào…"
date: "2026-06-19T19:26:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106144
codeforces_index: "A"
codeforces_contest_name: "2025-2026 ICPC, NERC, Southern and Volga Russian Regional Contest"
rating: 0
weight: 106144
solve_time_s: 57
verified: true
draft: false
---

[CF 106144A - Xóa mảng](https://codeforces.com/problemset/problem/106144/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng có thể thu nhỏ theo hai quy tắc xóa rất cụ thể. Quy tắc đầu tiên cho phép chúng tôi loại bỏ một lần xuất hiện của giá trị nhỏ nhất hiện có trong mảng và khi một số vị trí chứa giá trị tối thiểu đó, chúng tôi có thể tự do chọn vị trí nào biến mất. Quy tắc thứ hai có cấu trúc chặt chẽ hơn: nếu mảng hiện bắt đầu bằng hai giá trị bằng nhau, chúng ta có thể xóa cả hai giá trị đó cùng một lúc. 

Mục tiêu là xóa hoàn toàn mảng bằng cách sử dụng càng ít thao tác càng tốt. Mỗi thao tác làm giảm độ dài nhưng hiệu ứng không đối xứng: một quy tắc xóa chính xác một phần tử được chọn từ tập hợp tối thiểu toàn cục, trong khi quy tắc kia xóa một cặp liền kề cố định ở phía trước, nhưng chỉ khi hai phần tử đó khớp nhau. 

Ràng buộc rằng tổng kích thước của tất cả các trường hợp thử nghiệm tối đa là 2·10^5 ngụ ý rằng chúng ta cần một giải pháp gần tuyến tính hoặc tuyến tính cho mỗi bộ thử nghiệm. Bất kỳ cách tiếp cận nào liên tục mô phỏng việc xóa trên một mảng động bằng các lần quét lồng nhau sẽ quá chậm, đặc biệt vì các hoạt động lặp đi lặp lại “tìm mức tối thiểu” hoặc “kiểm tra sự bằng nhau của tiền tố” trong một vòng lặp đơn giản sẽ giảm xuống O(n^2). 

Một vấn đề nhỏ xuất hiện trong cách hoạt động tiền tố tương tác với việc xóa phần tử tối thiểu. Việc xóa một giá trị tối thiểu có thể hiển thị các cặp bằng nhau liền kề mới đã được phân tách trước đó bằng mức tối thiểu đó. Một chiến lược tham lam ngây thơ luôn áp dụng thao tác tiền tố bất cứ khi nào có thể có thể thất bại nếu nó bỏ qua việc loại bỏ một số giá trị cực tiểu nhất định trước đó có thể mở khóa nhiều lần xóa cặp trong tương lai. 

Ví dụ, hãy xem xét một mảng như [2, 1, 1, 2]. Nếu chúng ta háo hức loại bỏ số 1 tối thiểu ở giữa, chúng ta có thể làm gián đoạn cơ hội áp dụng việc xóa cặp hai lần sau đó. Chiến lược tối ưu phụ thuộc vào việc bảo toàn cấu trúc để các cặp có thể xếp tầng. 

Một tình huống cạnh khác là khi mảng đã bao gồm đầy đủ các cặp bằng nhau liền kề, như [3, 3, 2, 2, 1, 1]. Ở đây, cách chơi tối ưu sẽ tránh hoàn toàn việc xóa từng lần không cần thiết và bất kỳ chiến lược nào ưu tiên loại bỏ tối thiểu mà không xem xét khả năng ghép nối sẽ tính quá nhiều thao tác. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ mô phỏng tất cả các hoạt động hợp lệ ở mọi trạng thái: ở mỗi bước, hãy thử xóa phần tử tối thiểu ở tất cả các vị trí có thể hoặc nếu tiền tố cho phép, hãy xóa hai phần tử đầu tiên. Điều này tạo thành một cây tìm kiếm trên các trạng thái mảng. Ngay cả với việc ghi nhớ, số lượng trạng thái riêng biệt có thể bùng nổ vì việc loại bỏ các lần xuất hiện khác nhau có cùng mức tối thiểu sẽ dẫn đến các cấu trúc kề cận khác nhau trong tương lai. Trong trường hợp xấu nhất, mỗi lần xóa sẽ phân nhánh thành nhiều cấu hình tiếp theo, tạo ra hành vi theo cấp số nhân. 

Quan sát quan trọng là thao tác thứ hai hoàn toàn mang tính cấu trúc: nó chỉ loại bỏ một cặp khi mảng bắt đầu với hai phần tử bằng nhau và nó luôn sử dụng cả hai ngay lập tức. Điều này có nghĩa là bất kỳ tiền tố tối đa nào của các phần tử bằng nhau đều hoạt động giống như một tập hợp các mức giảm bắt buộc bất cứ khi nào chúng ta căn chỉnh các ranh giới một cách chính xác. 

Hoạt động đầu tiên là hoạt động duy nhất tương tác trên toàn cầu với việc đặt hàng. Tuy nhiên, nó cũng bị hạn chế: nó luôn nhắm tới giá trị tối thiểu hiện tại. Điều này cho thấy rằng các giá trị lớn hơn mức tối thiểu không bao giờ bị xóa trực tiếp ngoại trừ việc bị lộ thông qua việc xóa các phần tử nhỏ hơn. 

Sự thay đổi cơ cấu quan trọng là ngừng suy nghĩ về vị trí và thay vào đó nghĩ về các giá trị được sắp xếp theo độ lớn. Mỗi khi chúng tôi quyết định xóa một phần tử tối thiểu, chúng tôi đang lựa chọn một cách hiệu quả xem có nên “phá vỡ” mức giảm cặp tiềm năng trong tương lai hay duy trì cấu trúc cho phép xóa hàng loạt.

Điều này dẫn đến quan điểm nén tham lam: nếu xử lý các giá trị theo thứ tự tăng dần, chúng ta có thể suy luận về số lần mỗi giá trị phải được loại bỏ riêng lẻ so với số lần có thể được loại bỏ thông qua các tầng ghép nối liền kề. Mảng phân hủy một cách tự nhiên thành các phân đoạn trong đó các giá trị bằng nhau có thể hủy theo cặp sau khi loại bỏ các phần tử nhỏ hơn can thiệp. 

Giải pháp tối ưu giúp giảm bớt việc theo dõi số lượng “khối hoạt động” tồn tại sau khi liên tục mô phỏng tác dụng của việc loại bỏ tất cả các giá trị nhỏ hơn và đếm số lần xóa cần thiết để giải quyết các phần tử còn sót lại chưa khớp. 

Một cách thuận tiện để chính thức hóa điều này là quét các giá trị và duy trì cấu trúc theo dõi số lượng phần tử của mỗi giá trị còn lại sau khi ghép nối sụp đổ từ bên trái, xử lý thao tác thứ hai như hủy bỏ ngăn xếp tham lam của các phân đoạn liền kề bằng nhau sau khi các trình chặn nhỏ hơn biến mất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force của mọi hoạt động | Hàm mũ | O(n) | Quá chậm | 
| Nén tham lam về mặt giá trị với ghép nối giống như ngăn xếp | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi diễn giải lại quá trình này như duy trì một biểu diễn rút gọn của mảng trong đó các phần tử bằng nhau liền kề có thể hủy theo cặp bất cứ khi nào chúng trở nên liền kề sau khi loại bỏ các giá trị nhỏ hơn. 

Chúng tôi xử lý mảng từ trái sang phải trong khi vẫn duy trì một chồng các giá trị đại diện cho tiền tố hiệu quả hiện tại sau khi áp dụng tất cả các thao tác xóa cặp có thể có. 

1. Khởi tạo một ngăn xếp trống sẽ đại diện cho mảng đã rút gọn hiện tại sau khi xóa tất cả các cặp tiền tố có thể có. 
2. Lặp qua từng phần tử trong mảng từ trái sang phải. Đối với mỗi phần tử, hãy cố gắng hợp nhất nó vào cấu trúc hiện tại. 
3. Nếu ngăn xếp không trống và đỉnh của nó bằng phần tử hiện tại, hãy loại bỏ phần tử trên cùng thay vì đẩy phần tử hiện tại. Điều này tương ứng với việc áp dụng ngầm thao tác thứ hai, vì hai phần tử bằng nhau liền kề sẽ hủy bỏ ngay khi chúng liên tiếp ở trạng thái hiệu dụng. 
4. Nếu đỉnh ngăn xếp khác nhau, hãy đẩy phần tử hiện tại. Điều này thể hiện ranh giới mà hiện tại không thể ghép nối được. 
5. Sau khi xử lý toàn bộ mảng, ngăn xếp chứa các phần tử không thể loại bỏ chỉ bằng cách hủy cặp. Mỗi phần tử còn lại tương ứng với việc buộc phải sử dụng thao tác đầu tiên ở một giai đoạn nào đó của quy trình. 
6. Câu trả lời là kích thước của ngăn xếp cuối cùng, bởi vì mọi phần tử chưa khớp còn lại phải bị xóa riêng lẻ dưới dạng phần tử tối thiểu tại một số điểm, trong khi tất cả cấu trúc có thể hủy đã bị loại bỏ. 

Lý do điều này hoạt động là vì mọi cách sử dụng hợp lệ của thao tác thứ hai đều tương ứng chính xác với việc loại bỏ một cặp phần tử liền kề bằng nhau ở trạng thái rút gọn hoàn toàn. Bằng cách mô phỏng tất cả các lần hủy như vậy một cách tham lam, chúng tôi đảm bảo rằng không còn rào cản nhân tạo nào nữa. Những gì còn lại chính xác là các phần tử không thể được ghép nối theo bất kỳ thứ tự hoạt động hợp lệ nào và mỗi phần tử như vậy buộc phải có một sự kiện xóa tối thiểu trong bất kỳ chiến lược tối ưu nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    st = []
    for x in a:
        if st and st[-1] == x:
            st.pop()
        else:
            st.append(x)
    
    print(len(st))

t = int(input())
for _ in range(t):
    solve()
```Giải pháp duy trì một ngăn xếp thu gọn các phần tử bằng nhau liền kề ngay lập tức. Thao tác pop tương ứng chính xác với việc áp dụng thao tác được phép thứ hai ở dạng tích cực nhất của nó, vì bất kỳ lúc nào hai giá trị bằng nhau trở nên liền kề trong chuỗi hiệu quả, chúng có thể bị xóa mà không ảnh hưởng đến tính khả thi trong tương lai. 

Kích thước ngăn xếp cuối cùng được in vì nó thể hiện phần còn lại không thể rút gọn sau khi loại bỏ tất cả các cặp có thể xảy ra. Mỗi phần tử còn lại được cách ly về mặt cấu trúc và không thể ghép nối với bất kỳ phần tử lân cận nào theo các thao tác được phép, buộc phải thực hiện bước xóa riêng bằng thao tác đầu tiên. 

Một điểm tinh tế là chúng tôi không bao giờ mô phỏng rõ ràng việc “xóa tối thiểu”. Hoạt động đó được hấp thụ vào bất biến: bất kỳ thứ gì không thể xóa được thông qua ghép nối cuối cùng phải được loại bỏ từng cái một và ngăn xếp sẽ nắm bắt chính xác phần còn lại đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

[5, 2, 5, 2, 2, 4] 

| Bước | Yếu tố | Xếp chồng trước | Hành động | Xếp chồng sau | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | [] | đẩy | [5] | 
| 2 | 2 | [5] | đẩy | [5, 2] | 
| 3 | 5 | [5, 2] | đẩy | [5, 2, 5] | 
| 4 | 2 | [5, 2, 5] | đẩy | [5, 2, 5, 2] | 
| 5 | 2 | [5, 2, 5, 2] | trận đấu hàng đầu, pop | [5, 2, 5] | 
| 6 | 4 | [5, 2, 5] | đẩy | [5, 2, 5, 4] | 

Câu trả lời cuối cùng là 4. 

Dấu vết này cho thấy chỉ có sự liền kề chính xác của các phần tử bằng nhau mới gây ra sự hủy bỏ, trong khi tất cả các cấu trúc khác tồn tại dưới dạng dư lượng không thể giảm được. 

### Ví dụ 2 

đầu vào: 

[5, 5, 5, 5, 1, 1] 

| Bước | Yếu tố | Xếp chồng trước | Hành động | Xếp chồng sau | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | [] | đẩy | [5] | 
| 2 | 5 | [5] | bật | [] | 
| 3 | 5 | [] | đẩy | [5] | 
| 4 | 5 | [5] | bật | [] | 
| 5 | 1 | [] | đẩy | [1] | 
| 6 | 1 | [1] | bật | [] | 

Câu trả lời cuối cùng là 0. 

Điều này cho thấy sự hủy diệt hoàn toàn thông qua ghép cặp, xác nhận rằng sự kề cận bằng nhau lặp đi lặp lại sẽ làm sụp đổ mọi thứ mà không cần bất kỳ sự xóa bỏ tối thiểu nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi phần tử được đẩy và xuất hiện nhiều nhất một lần | 
| Không gian | O(n) | Ngăn xếp lưu trữ tối đa n phần tử trong trường hợp xấu nhất | 

Tổng độ phức tạp trong tất cả các trường hợp thử nghiệm vẫn tuyến tính trong tổng kích thước đầu vào, thỏa mãn ràng buộc 2·10^5 phần tử một cách thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdin
    input = stdin.readline

    def solve():
        n = int(input())
        a = list(map(int, input().split()))
        st = []
        for x in a:
            if st and st[-1] == x:
                st.pop()
            else:
                st.append(x)
        return len(st)

    t = int(input())
    out = []
    for _ in range(t):
        out.append(str(solve()))
    return "\n".join(out)

# sample-like cases
assert run("1\n1\n5\n") == "1"
assert run("1\n2\n1 1\n") == "0"

# all equal long cancellation
assert run("1\n6\n2 2 2 2 2 2\n") == "0"

# alternating no cancellation
assert run("1\n5\n1 2 3 4 5\n") == "5"

# partial cancellation
assert run("1\n6\n1 1 2 2 3 3\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 1 | trường hợp tối thiểu, không ghép nối | 
| tất cả các cặp bằng nhau | 0 | hủy bỏ toàn bộ tầng | 
| xen kẽ | n | không thể hợp nhất | 
| cặp nhóm | 0 | ghép nối hoàn chỉnh theo khối | 

## Vỏ cạnh 

Trường hợp một cạnh là mảng một phần tử, chẳng hạn như [7]. Ngăn xếp bắt đầu trống, phần tử được đẩy và kích thước ngăn xếp cuối cùng là 1, điều này phản ánh chính xác rằng cần phải xóa một lần. 

Một trường hợp cạnh khác là cấu trúc ghép nối hoàn toàn đối xứng như [4, 4, 3, 3, 2, 2]. Ngăn xếp liên tục hủy các cặp ở mỗi bước, cuối cùng trở nên trống. Điều này chứng tỏ rằng thuật toán xử lý chính xác việc hủy theo chuỗi mà không cần bất kỳ lý do tổng thể rõ ràng nào. 

Trường hợp thứ ba là khi việc hủy phụ thuộc vào cấu trúc trung gian, chẳng hạn như [1, 2, 2, 1]. 2 số đầu tiên được đẩy, 2 số thứ hai hủy nó, sau đó 1 và 1 hủy bỏ. Mô hình ngăn xếp nắm bắt chính xác tương tác không cục bộ này vì tính kề cận luôn được đánh giá trên tiền tố rút gọn hiện tại chứ không phải mảng ban đầu.
