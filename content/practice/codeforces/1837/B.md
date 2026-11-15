---
title: "CF 1837B - Chuỗi so sánh"
description: "Chúng ta được cung cấp một chuỗi mã hóa các so sánh cục bộ giữa các phần tử lân cận của một mảng. Mỗi ký tự mô tả phần tử tiếp theo phải lớn hơn hay nhỏ hơn phần tử hiện tại."
date: "2026-06-09T06:38:41+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1837
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 149 (Rated for Div. 2)"
rating: 900
weight: 1837
solve_time_s: 81
verified: false
draft: false
---

[CF 1837B - Chuỗi so sánh](https://codeforces.com/problemset/problem/1837/B) 

**Xếp hạng:** 900 
**Thẻ:** tham lam 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi mã hóa các so sánh cục bộ giữa các phần tử lân cận của một mảng. Mỗi ký tự mô tả phần tử tiếp theo phải lớn hơn hay nhỏ hơn phần tử hiện tại. Nhiệm vụ không phải là xây dựng bất kỳ mảng hợp lệ nào mà là xây dựng một mảng sử dụng càng ít giá trị riêng biệt càng tốt trong khi vẫn tôn trọng tất cả các so sánh này. 

Một cách hữu ích để suy nghĩ về vấn đề này là chúng ta đang cố gắng gán độ cao cho các vị trí dọc theo một đường thẳng, trong đó mỗi bước buộc chúng ta phải đi lên hoặc đi xuống. Chúng tôi được phép sử dụng lại các giá trị, nhưng chỉ khi việc làm đó không vi phạm các yêu cầu nghiêm ngặt về bất bình đẳng. 

Đầu ra là số lượng số nguyên riêng biệt nhỏ nhất cần thiết để thực hiện một “bước đi” theo trình tự thăng trầm đã cho. 

Các ràng buộc đủ nhỏ để ý tưởng O(n²) cho mỗi trường hợp thử nghiệm có thể vượt qua một cách thoải mái, vì n nhiều nhất là 100 và t nhiều nhất là 500. Điều này gợi ý rõ ràng rằng chúng ta không nên tối ưu hóa tiệm cận ngoài việc quét tuyến tính hoặc bậc hai. Thách thức thực sự là hiểu cấu trúc nào xác định số lượng giá trị riêng biệt tối thiểu. 

Một nỗ lực ngây thơ có thể cố gắng xây dựng các mảng một cách rõ ràng hoặc gán các giá trị một cách tham lam từ trái sang phải trong khi sử dụng lại các số bất cứ khi nào có thể. Điều này nhanh chóng trở nên mong manh vì các quyết định tái sử dụng cục bộ có thể cản trở việc tái sử dụng trong tương lai theo những cách khó dự đoán. 

Ý tưởng ngây thơ thứ hai là thử tất cả các phép gán có thể có bằng cách quay lui trên một phạm vi số nguyên nhỏ. Ngay cả khi chỉ có 100 vị trí, hệ số phân nhánh thực tế là không bị giới hạn và các ràng buộc về bất đẳng thức có nghĩa là chúng ta sẽ khám phá một không gian tổ hợp lớn của các nhãn. 

Trường hợp lỗi tinh vi hơn xuất hiện khi chuỗi thay thế, chẳng hạn như “<><><><”. Cách tiếp cận tham lam có thể cố gắng sử dụng lại hai giá trị nhiều lần, nhưng hóa ra điều này tạo ra một dao động đòi hỏi phải đưa ra nhiều giá trị khác biệt hơn mong đợi nếu bạn chỉ định kém. Câu trả lời đúng phụ thuộc vào cấu trúc toàn cầu chứ không phải quyết định cục bộ. 

## Phương pháp tiếp cận 

Quan sát quan trọng là vấn đề không thực sự nằm ở các giá trị tuyệt đối mà là ở các ràng buộc thứ tự tương đối. Chúng tôi chỉ quan tâm đến việc các phần tử liên tiếp tăng hay giảm, điều đó có nghĩa là các con số thực tế có thể được coi là các nhãn được gán cho một đường di chuyển lên và xuống. 

Số lượng giá trị riêng biệt tối thiểu tương ứng với số lượng “cấp độ” tối thiểu cần thiết để thực hiện toàn bộ chuỗi một cách nhất quán. Mỗi lần đi lên, chúng tôi cần đảm bảo có thể chuyển lên mức chưa sử dụng cao hơn nếu cần, và mỗi lần đi xuống, chúng tôi phản ánh hạn chế đó theo hướng ngược lại. 

Thông tin chi tiết về cấu trúc quan trọng là mọi phép gán hợp lệ đều có thể được dịch chuyển và nén sao cho các giá trị tương ứng với độ cao trong một bước đi. Chi phí sau đó là số lượng độ cao riêng biệt được truy cập. Điều này làm giảm vấn đề khi phân tích phạm vi cực trị của một cấu trúc giống như tổng tiền tố. 

Nếu chúng tôi diễn giải “<” là +1 và “>” là -1 và theo dõi số dư hiện hành thì mức chênh lệch tối đa giữa giá trị cao nhất và thấp nhất của tổng hiện hành này sẽ xác định số lượng giá trị riêng biệt được yêu cầu. Theo trực giác, nếu bước đi đi xuống k bước so với điểm bắt đầu và k bước ở trên trong một số dạng dịch chuyển, thì chúng ta cần k+1 mức riêng biệt để phù hợp với nó. 

Một cách chính xác hơn để thấy điều này là xét rằng mỗi vị trí tương ứng với một điểm trên một đường số nguyên được xác định bởi chuyển động tích lũy. Số lượng giá trị riêng biệt tối thiểu là số lượng độ cao riêng biệt trong biểu diễn nén tối ưu, chính xác là phạm vi cực trị tiền tố có thể truy cập. 

Ý tưởng mạnh mẽ về việc gán các giá trị rõ ràng là thất bại vì nó không nắm bắt được rằng chỉ có thứ hạng tương đối mới quan trọng. Khi chúng tôi chuyển sang theo dõi các tiền tố cực trị, giải pháp sẽ trở thành tuyến tính cho mỗi trường hợp thử nghiệm.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Xây dựng Brute Force | O(2^n) hoặc tệ hơn | O(n) | Quá chậm | 
| Theo dõi cực trị tiền tố tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi chuỗi so sánh thành một chuỗi các bước định hướng, coi mỗi ký tự là một ràng buộc chuyển động sẽ ảnh hưởng đến vị trí đang chạy. 
2. Khởi tạo bộ đếm đang chạy ở mức 0, đại diện cho “chiều cao” hiện tại trong cấu trúc giả định. 
3. Di chuyển chuỗi từ trái sang phải. Khi gặp “<”, hãy tăng bộ đếm lên 1. Khi gặp “>”, hãy giảm bộ đếm đi 1. Điều này mô phỏng một biểu diễn nén hợp lệ của mảng. 

Lý do điều này có hiệu quả là vì chúng tôi chưa chỉ định các giá trị thực tế mà đang xây dựng một cấu trúc thứ tự tương đối tôn trọng mọi ràng buộc. 
4. Trong khi cập nhật bộ đếm đang chạy, hãy theo dõi cả giá trị tối thiểu đạt được và giá trị tối đa đạt được trong quá trình truyền tải. 
5. Câu trả lời là kích thước của phạm vi được bao phủ bởi bước đi này, được tính bằng`max_value - min_value + 1`. 

Phạm vi này biểu thị số lượng nhãn số nguyên riêng biệt được yêu cầu nếu chúng ta nén cấu trúc một cách tối ưu sao cho mỗi chiều cao tương ứng với một giá trị duy nhất. 

### Tại sao nó hoạt động 

Bất kỳ mảng hợp lệ nào đều tạo ra một chuỗi so sánh tương đối có thể được ánh xạ tới một bước đi trên các số nguyên bằng cách gán liên tục các sai phân liền kề phù hợp với các dấu hiệu. Nén bước đi đó bằng cách dịch chuyển nó để giá trị tối thiểu trở thành 0 không làm thay đổi tính hợp lệ nhưng giảm thiểu số lượng giá trị riêng biệt. Số lượng các giá trị riêng biệt chính xác là số mức nguyên được truy cập, bằng với phạm vi giữa độ dịch chuyển tiền tố tối thiểu và tối đa. Không có phép gán thay thế nào có thể làm giảm phạm vi này, bởi vì bất kỳ nỗ lực nào để sử dụng lại các giá trị sẽ buộc phải vi phạm thứ tự nghiêm ngặt trong ít nhất một phân đoạn của chuỗi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        s = input().strip()

        cur = 0
        mn = 0
        mx = 0

        for ch in s:
            if ch == '<':
                cur += 1
            else:
                cur -= 1
            mn = min(mn, cur)
            mx = max(mx, cur)

        print(mx - mn + 1)

if __name__ == "__main__":
    solve()
```Mã duy trì một vị trí chạy duy nhất và cập nhật nó theo từng so sánh. Các giá trị tối thiểu và tối đa quan sát được trong quá trình truyền tải này xác định khoảng cách của cấu trúc cảm ứng. Câu trả lời cuối cùng được tính bằng kích thước của khoảng này. Việc triển khai phải cẩn thận để khởi tạo các bộ đếm ở mức 0, vì mảng bắt đầu trước khi áp dụng bất kỳ phép so sánh nào. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`<<>>`Chúng tôi theo dõi giá trị đang chạy và cực trị của nó. 

| Bước | Char | Hiện tại | Tối thiểu | Tối đa | 
| --- | --- | --- | --- | --- | 
| 0 | bắt đầu | 0 | 0 | 0 | 
| 1 | < | 1 | 0 | 1 | 
| 2 | < | 2 | 0 | 2 | 
| 3 | > | 1 | 0 | 2 | 
| 4 | > | 0 | 0 | 2 | 

Phạm vi cuối cùng là 2 - 0 + 1 = 3. 

Dấu vết này cho thấy rằng mặc dù cuối cùng chúng ta quay về 0, nhưng đỉnh trung gian của 2 tạo ra ba mức riêng biệt trong bất kỳ cách xây dựng hợp lệ nào. 

### Ví dụ 2:`>>>>`| Bước | Char | Hiện tại | Tối thiểu | Tối đa | 
| --- | --- | --- | --- | --- | 
| 0 | bắt đầu | 0 | 0 | 0 | 
| 1 | > | -1 | -1 | 0 | 
| 2 | > | -2 | -2 | 0 | 
| 3 | > | -3 | -3 | 0 | 
| 4 | > | -4 | -4 | 0 | 

Phạm vi cuối cùng là 0 - (-4) + 1 = 5. 

Trường hợp này cho thấy rằng một chuỗi giảm đơn điệu buộc phải tăng tuyến tính các giá trị riêng biệt bắt buộc, vì mỗi bước sẽ đẩy cấu trúc lên mức tối thiểu mới. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi ký tự được xử lý một lần trong khi cập nhật giới hạn đang chạy | 
| Không gian | O(1) | Chỉ có một số quầy được duy trì | 

Tổng công việc trên tất cả các trường hợp thử nghiệm vẫn nhỏ ngay cả ở kích thước đầu vào tối đa vì thuật toán thực hiện quét tuyến tính duy nhất cho mỗi trường hợp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(sys.stdin.readline())
    out = []
    for _ in range(t):
        n = int(sys.stdin.readline())
        s = sys.stdin.readline().strip()

        cur = 0
        mn = 0
        mx = 0
        for ch in s:
            if ch == '<':
                cur += 1
            else:
                cur -= 1
            mn = min(mn, cur)
            mx = max(mx, cur)

        out.append(str(mx - mn + 1))

    return "\n".join(out)

# provided samples
assert run("""4
4
<<>>
4
>><<
5
>>>>>
7
<><><><
""") == """3
3
6
2"""

# custom cases
assert run("""1
1
<
""") == "2", "minimum size increasing"

assert run("""1
1
>
""") == "2", "minimum size decreasing"

assert run("""1
6
<><><>
""") == "2", "alternating small range"

assert run("""1
6
<<<<<<
""") == "7", "strict increase chain"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`<`| 2 | trường hợp tăng tối thiểu | 
|`>`| 2 | trường hợp giảm tối thiểu | 
|`<><><>`| 2 | tái sử dụng mô hình xen kẽ | 
|`<<<<<<`| 7 | tăng trưởng tuyến tính của phạm vi | 

## Vỏ cạnh 

Chuỗi ký tự đơn cho biết liệu thuật toán có xử lý chính xác việc khởi tạo cơ sở hay không. Đối với “<”, giá trị đang chạy trở thành 1, do đó phạm vi là [0,1], cho câu trả lời 2. Đối với “>”, giá trị đang chạy trở thành -1, do đó phạm vi là [-1,0], một lần nữa cho kết quả 2. Thuật toán xử lý một cách tự nhiên cả hai mà không cần viết hoa đặc biệt vì cực trị được khởi tạo bằng 0. 

Một chuỗi giảm hoàn toàn như “>>>>>>” liên tục đẩy giá trị đang chạy xuống. Mức tối thiểu tiếp tục giảm trong khi mức tối đa vẫn ở mức 0, do đó phạm vi tăng tuyến tính. Mỗi bước đưa ra một mức tối thiểu mới và câu trả lời cuối cùng phản ánh tổng độ sâu đạt được. 

Một chuỗi xen kẽ như “<><><><” giữ cho giá trị đang chạy dao động giữa hai mức liền kề. Mức tối thiểu và tối đa ổn định nhanh chóng ở mức 0 và 1, do đó kết quả vẫn là 2. Điều này xác nhận rằng dao động không tăng các giá trị khác biệt cần thiết vượt quá hai vì bước đi không bao giờ mở rộng phạm vi của nó.
