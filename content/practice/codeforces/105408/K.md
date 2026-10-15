---
title: "CF 105408K - Đóng bếp"
description: "Chúng tôi được cung cấp một căn bếp có sẵn một số nguyên liệu cố định. Mỗi thành phần bắt đầu với một số lượng. Có một thực đơn gồm các món ăn và mỗi món ăn đều tiêu tốn một lượng nhất định các nguyên liệu này khi chế biến."
date: "2026-06-24T23:10:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105408
codeforces_index: "K"
codeforces_contest_name: "2024 ICPC Gran Premio de Mexico Repechaje"
rating: 0
weight: 105408
solve_time_s: 77
verified: false
draft: false
---

[CF 105408K - Đóng bếp](https://codeforces.com/problemset/problem/105408/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một căn bếp có sẵn một số nguyên liệu cố định. Mỗi thành phần bắt đầu với một số lượng. Có một thực đơn gồm các món ăn và mỗi món ăn đều tiêu tốn một lượng nhất định các nguyên liệu này khi chế biến. Cuối cùng, có một chuỗi đơn đặt hàng của khách hàng và mỗi đơn hàng là một danh sách các món ăn phải được chuẩn bị cùng nhau. 

Đơn đặt hàng phải được xử lý nghiêm ngặt theo thứ tự nhất định. Đối với mỗi đơn hàng, chúng tôi kiểm tra xem nguyên liệu dự trữ hiện tại có đủ để chuẩn bị mọi món ăn được yêu cầu trong đơn hàng đó hay không. Nếu đúng như vậy, chúng tôi khấu trừ các thành phần đã sử dụng và tiếp tục. Lần đầu tiên chúng tôi gặp phải một đơn hàng không thể chuẩn bị đầy đủ, nhà bếp dừng ngay lập tức và chúng tôi báo cáo số lượng đơn hàng đã được hoàn thành thành công trước thất bại này. 

Các ràng buộc rất nhỏ: tối đa 100 nguyên liệu, 100 món ăn và 100 đơn hàng. Mỗi món ăn có thể phụ thuộc vào tối đa 100 nguyên liệu và mỗi đơn hàng có thể chứa tối đa 100 món. Kích thước này ngay lập tức gợi ý rằng việc tính toán lại việc sử dụng thành phần từ đầu cho mỗi đơn hàng là khả thi. Ngay cả trong trường hợp xấu nhất, chúng tôi đang thực hiện theo thứ tự vài triệu thao tác nguyên thủy, nằm trong giới hạn thoải mái cho giải pháp 1 giây trong Python. 

Một trường hợp thất bại tinh vi đối với lối suy luận ngây thơ xuất phát từ việc quên rằng việc sử dụng nguyên liệu được tích lũy trong các món ăn trong cùng một đơn hàng và giữa các đơn hàng trước đó. Ví dụ: nếu thành phần 1 có số lượng 5 và đơn hàng đầu tiên sử dụng 3 đơn vị và đơn hàng thứ hai sử dụng thêm 3 đơn vị, thì đơn hàng thứ hai phải thất bại ngay cả khi mỗi món ăn riêng lẻ có vẻ hợp lý nếu đứng riêng lẻ. Một sai lầm phổ biến khác là áp dụng một phần mệnh lệnh trước khi phát hiện ra sự thiếu sót; toàn bộ đơn hàng phải thành công hoặc thất bại về mặt nguyên tử. 

## Phương pháp tiếp cận 

Cách trực tiếp để xử lý vấn đề này là mô phỏng quy trình chính xác như được mô tả. Chúng tôi lưu trữ hàng tồn kho hiện tại của các thành phần. Đối với mỗi đơn hàng, chúng tôi tính toán tổng lượng nguyên liệu tiêu thụ cần thiết bằng cách cộng tổng phần đóng góp của mỗi món ăn trong đơn hàng đó. Sau khi biết tổng nhu cầu cho đơn hàng đó, chúng tôi sẽ kiểm tra xem tất cả nguyên liệu có đủ số lượng hay không. Nếu có, chúng tôi sẽ trừ chúng khỏi kho; nếu không, chúng tôi dừng lại. 

Cách tiếp cận bạo lực này là đúng vì nó phản ánh quá trình được xác định trong báo cáo vấn đề. Mối lo ngại về tính kém hiệu quả xuất phát từ việc tính toán lại việc sử dụng thành phần nhiều lần. Nếu chúng ta mở rộng mọi đơn hàng thành tất cả các món ăn và từng món ăn thành các yêu cầu về thành phần thì với mỗi đơn hàng, chúng ta có thể chạm tới 100 món ăn và mỗi món ăn có tới 100 nguyên liệu, tổng cộng khoảng 1e6 thao tác, vốn đã nhỏ. Vì chúng tôi chỉ xử lý mỗi đơn hàng một lần và dừng ở lần thất bại đầu tiên nên không có việc tính toán lại lặp đi lặp lại theo các bước thời gian có thể đẩy việc này vượt quá giới hạn. 

Quan sát quan trọng là không có cấu trúc động hoặc tối ưu hóa nào cần thiết ngoài việc tổng hợp. Mỗi đơn hàng đều độc lập ngoại trừ khoảng không quảng cáo chung có thể thay đổi, vì vậy chúng tôi chỉ cần một bước tích lũy đơn giản cho mỗi đơn hàng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force | O(O × món ăn trung bình × nguyên liệu mỗi món) | O(M×N) | Đã chấp nhận | 
| Mức sử dụng được tính toán trước cho mỗi món ăn + mô phỏng | O(O × món ăn trung bình × N) | O(M×N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi chuyển đổi mô tả món ăn thành một cấu trúc cho phép truy cập nhanh vào các yêu cầu về thành phần. Sau đó, chúng tôi xử lý liên tục từng đơn hàng cho đến khi hoàn thành tất cả hoặc gặp thất bại.

1. Đọc số lượng ban đầu của tất cả các thành phần và lưu trữ chúng trong một mảng. Điều này thể hiện tình trạng hiện tại của hàng tồn kho trong nhà bếp. 
2. Đối với mỗi món ăn, hãy lưu trữ một danh sách hoặc mảng có kích thước N thể hiện số lượng mỗi thành phần được yêu cầu. Điều này tránh việc phân tích cú pháp lặp đi lặp lại trong quá trình xử lý đơn hàng và biến mỗi món ăn thành một đối tượng giống như vectơ trực tiếp. 
3. Với mỗi đơn hàng, tạo một mảng tạm thời`need`có kích thước N được khởi tạo bằng 0. Mảng này tổng hợp tổng nhu cầu thành phần cho đơn hàng đó. 
4. Đối với mỗi món ăn trong đơn hàng, hãy thêm các yêu cầu về thành phần của món đó vào`need`. Bước này chuyển đổi danh sách các món ăn thành một vectơ yêu cầu tổng hợp duy nhất. 
5. Sau khi chế biến xong tất cả các món theo thứ tự, hãy so sánh`need`so với hàng tồn kho hiện tại. Nếu bất kỳ yêu cầu thành phần nào vượt quá lượng hàng sẵn có, hãy dừng ngay lập tức và trả lại số lượng đơn hàng đã xử lý thành công cho đến nay. 
6. Nếu thứ tự khả thi thì trừ đi`need`từ kho và tiếp tục đặt hàng tiếp theo. 

Ý tưởng chính là mỗi đơn hàng được coi là một giao dịch không thể chia nhỏ: trước tiên chúng tôi tính toán toàn bộ chi phí của nó và chỉ sau đó mới áp dụng nó. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, hàng tồn kho thể hiện chính xác những gì còn lại sau khi xử lý tất cả các đơn đặt hàng thành công trước đó. Mỗi đơn hàng được đánh giá theo trạng thái này bằng cách sử dụng tổng hợp đầy đủ các yêu cầu của nó. Vì chúng tôi không bao giờ áp dụng một phần đơn hàng và vì các yêu cầu về món ăn là cố định và độc lập nên quyết định cho mỗi đơn hàng là chính xác đối với trạng thái hiện tại. Điều này duy trì tính bất biến là hàng tồn kho luôn chỉ phản ánh những đơn hàng đã được hoàn thành đầy đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N, M, O = map(int, input().split())
    stock = list(map(int, input().split()))

    dishes = [None] * M
    for i in range(M):
        k = int(input())
        req = [0] * N
        for _ in range(k):
            ing, amt = map(int, input().split())
            req[ing - 1] = amt
        dishes[i] = req

    done = 0

    for _ in range(O):
        parts = list(map(int, input().split()))
        o = parts[0]
        order_dishes = parts[1:]

        need = [0] * N

        for d in order_dishes:
            req = dishes[d - 1]
            for i in range(N):
                if req[i]:
                    need[i] += req[i]

        ok = True
        for i in range(N):
            if need[i] > stock[i]:
                ok = False
                break

        if not ok:
            break

        for i in range(N):
            stock[i] -= need[i]

        done += 1

    print(done)

if __name__ == "__main__":
    solve()
```Giải pháp tính toán trước mỗi món ăn dưới dạng vectơ thành phần có độ dài cố định để việc đánh giá thứ tự trở thành một bước tích lũy đơn giản. Chi tiết triển khai quan trọng là tránh sửa đổi khoảng không quảng cáo trong khi kiểm tra tính khả thi; chúng tôi chỉ trừ đi sau khi xác nhận toàn bộ đơn hàng có thể được đáp ứng. 

Một điểm tinh tế khác là lập chỉ mục: các món ăn và nguyên liệu đều có đầu vào dựa trên 1, vì vậy cả hai đều được chuyển đổi nội bộ thành dựa trên 0. Việc kiểm tra tính khả thi phải diễn ra trước bất kỳ đột biến nào của mảng hàng tồn kho, nếu không, đơn hàng không thành công sẽ tiêu tốn tài nguyên một cách không chính xác. 

## Ví dụ đã hoạt động 

Hãy xem xét một kịch bản đơn giản hóa với một thành phần và ba đơn hàng. 

### Dấu vết mẫu 

Chúng tôi theo dõi hàng tồn kho và xử lý đơn hàng. 

| Bước | Kho | Đặt hàng | Bắt buộc | Khả thi | Hàng Mới | 
| --- | --- | --- | --- | --- | --- | 
| 1 | [10] | [1, 2] | [3 + 4 = 7] | Có | [3] | 
| 2 | [3] | [1] | [3] | Có | [0] | 
| 3 | [0] | [1] | [3] | Không | dừng lại | 

Quá trình này cho thấy mức tiêu thụ tích lũy qua các đơn đặt hàng cho đến khi lượng hàng tồn kho không đủ. 

Điều này xác nhận rằng thuật toán xử lý chính xác sự suy giảm tích lũy chứ không chỉ tính khả thi theo từng đơn hàng. 

### Lại một dấu vết thất bại sớm 

| Bước | Kho | Đặt hàng | Bắt buộc | Khả thi | Hàng Mới | 
| --- | --- | --- | --- | --- | --- | 
| 1 | [2, 2] | [1] | [3, 1] | Không | dừng lại | 

Ở đây, đơn hàng đầu tiên thất bại ngay lập tức, do đó không có sự thay đổi hàng tồn kho nào xảy ra và câu trả lời là 0. Điều này xác minh tính chất nguyên tử của việc xử lý đơn hàng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(O × M × N) trong trường hợp xấu nhất | Mỗi đơn hàng lặp lại các món ăn của nó và mỗi món ăn đóng góp tới N thành phần cập nhật và kiểm tra | 
| Không gian | O(M × N + N) | Kho lưu trữ các món ăn yêu cầu và lượng hàng hiện có | 

Cho rằng tất cả các giới hạn tối đa là 100, tổng công việc theo thứ tự các thao tác 1e6, phù hợp thoải mái với các hạn chế về thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return str(solve())

# Note: adapt solve to return value for testing
```Một khai thác thử nghiệm đã được sửa chữa giả định`solve()`trả lời câu trả lời:```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return str(solve())

# Since our solve prints, we redefine it here for tests
def solve():
    N, M, O = map(int, input().split())
    stock = list(map(int, input().split()))

    dishes = [None] * M
    for i in range(M):
        k = int(input())
        req = [0] * N
        for _ in range(k):
            ing, amt = map(int, input().split())
            req[ing - 1] = amt
        dishes[i] = req

    done = 0
    for _ in range(O):
        parts = list(map(int, input().split()))
        o = parts[0]
        order_dishes = parts[1:]

        need = [0] * N
        for d in order_dishes:
            req = dishes[d - 1]
            for i in range(N):
                need[i] += req[i]

        for i in range(N):
            if need[i] > stock[i]:
                print(done)
                return done

        for i in range(N):
            stock[i] -= need[i]

        done += 1

    print(done)
    return done

# provided samples (formatting-dependent; conceptual placeholders)
# assert run(...) == "1"
# assert run(...) == "3"

# custom cases
assert run("1 1 1\n5\n1\n1 5\n") == "1", "single perfect match"
assert run("1 1 1\n5\n1\n1 6\n") == "0", "immediate failure"
assert run("2 1 2\n5 5\n1\n1 1 1\n1 1\n") == "1", "depletion across orders"
assert run("2 2 2\n5 5\n1\n1 1 3\n1\n1 1\n") == "1", "second order fails"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trận đấu hoàn hảo duy nhất | 1 | thực hiện thành công cơ bản | 
| thất bại ngay lập tức | 0 | thất bại trong lần đặt hàng đầu tiên | 
| cạn kiệt trên các đơn đặt hàng | 1 | sử dụng tài nguyên tích lũy | 
| lệnh thứ hai thất bại | 1 | dừng ở lệnh không hợp lệ đầu tiên | 

## Vỏ cạnh 

Trường hợp cạnh thứ nhất là khi đơn hàng đầu tiên đã vượt quá lượng hàng sẵn có. Trong trường hợp này, thuật toán thực hiện kiểm tra tính khả thi, phát hiện sự thiếu hụt và trả về 0 mà không sửa đổi hàng tồn kho. Bất biến mà hàng tồn kho chỉ phản ánh các đơn hàng đã hoàn thành đảm bảo tính chính xác vì không xảy ra khấu trừ một phần. 

Một trường hợp khác phát sinh khi nhiều món ăn trong một đơn hàng tổng cộng vượt quá số lượng tồn kho mặc dù mỗi món ăn riêng lẻ đều có giá cả phải chăng. Thuật toán tổng hợp tất cả các đóng góp trước khi kiểm tra, do đó lỗi được phát hiện ở cấp độ đơn hàng thay vì theo từng món ăn. Điều này ngăn chặn việc trừ sớm không chính xác. 

Trường hợp cuối cùng là khi một đơn hàng làm cạn kiệt chính xác một thành phần. Vì sự so sánh là nghiêm ngặt (`need[i] > stock[i]`), sự bình đẳng được cho phép và sau đó cổ phiếu trở thành 0. Điều này duy trì tính chính xác cho các đơn đặt hàng tiếp theo yêu cầu thành phần đó, thành phần này sẽ thất bại trừ khi chúng không yêu cầu thành phần nào.
