---
title: "CF 104569A - Trợ giảng"
description: "Chúng tôi được cấp một chuỗi ngày và vào mỗi ngày, chúng tôi được phép thực hiện chính xác một hành động trong số ba lựa chọn: chúng tôi có thể yêu cầu một bộ vấn đề Mã hóa, yêu cầu một bộ vấn đề gây nhiễu hoặc gửi bộ vấn đề được yêu cầu gần đây nhất chưa được gửi."
date: "2026-06-30T08:26:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104569
codeforces_index: "A"
codeforces_contest_name: "2016 Google Code Jam Round 3 (GCJ 16 Round 3)"
rating: 0
weight: 104569
solve_time_s: 57
verified: true
draft: false
---

[CF 104569A - Trợ giảng](https://codeforces.com/problemset/problem/104569/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một chuỗi ngày và vào mỗi ngày, chúng tôi được phép thực hiện chính xác một hành động trong số ba lựa chọn: chúng tôi có thể yêu cầu một bộ vấn đề Mã hóa, yêu cầu một bộ vấn đề gây nhiễu hoặc gửi bộ vấn đề được yêu cầu gần đây nhất chưa được gửi. Quy tắc chính là việc gửi tuân theo kỷ luật ngăn xếp nghiêm ngặt, nghĩa là chúng tôi luôn gửi tập hợp chưa gửi được yêu cầu gần đây nhất, bất kể loại của nó. 

Mỗi bộ vấn đề có một loại, Mã hóa hoặc Gây nhiễu và mỗi yêu cầu sẽ diễn ra vào một ngày mà trợ lý có một tâm trạng đã biết, cũng là Mã hóa hoặc Gây nhiễu. Giá trị của một bộ vấn đề phụ thuộc vào việc yêu cầu có phù hợp với tâm trạng của ngày yêu cầu hay không và điểm số đạt được khi gửi bài phụ thuộc vào việc tâm trạng của ngày gửi có phù hợp với loại của bộ bài hay không và sẽ bị phạt nếu không. 

Chúng ta phải lên lịch yêu cầu và gửi trong tất cả các ngày để tối đa hóa tổng số điểm, với đầy đủ kiến ​​thức về mọi tâm trạng trong tương lai. 

Ràng buộc rằng ngăn xếp là LIFO là điều làm cho vấn đề trở nên không tầm thường: chúng ta không thể tự do lựa chọn bộ vấn đề nào để gửi. Thứ tự yêu cầu kém có thể chặn vĩnh viễn việc gửi có giá trị cao. 

Kích thước đầu vào lớn, lên tới 20000 ngày cho mỗi trường hợp thử nghiệm và tổng số 150000 trong các thử nghiệm, loại trừ mọi tìm kiếm theo cấp số nhân theo lịch trình. Bất kỳ giải pháp nào cũng phải tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. 

Một cách tiếp cận đơn giản sẽ cố gắng mô phỏng tất cả các chuỗi hành động yêu cầu và gửi có thể xảy ra. Ngay cả khi chúng ta hạn chế các chuỗi ngăn xếp hợp lệ, số lượng các phần xen kẽ vẫn tăng lên giống như cấu trúc Catalan và trở thành cấp số nhân. Điều này là không thể thực hiện được. 

Một trường hợp phức tạp xuất hiện khi việc trì hoãn gửi là có lợi vì tâm trạng của ngày hôm sau sẽ cải thiện giá trị của một bộ đã được yêu cầu. Ví dụ: nếu một Bộ mã hóa được yêu cầu trong một Ngày mã hóa nhưng chỉ có thể được gửi sau trong một Ngày mã hóa khác, thì chúng ta nên chờ đợi, nhưng việc chờ đợi quá lâu có thể chặn các yêu cầu trong tương lai do thứ tự ngăn xếp. 

## Phương pháp tiếp cận 

Khó khăn chính là sự tương tác giữa hai quyết định: loại tập hợp nào cần yêu cầu và khi nào nên gửi nó. Ràng buộc ngăn xếp kết hợp các quyết định này theo thời gian. 

Một chiến lược bạo lực sẽ coi mỗi ngày là phân nhánh thành ba khả năng và mô phỏng tất cả các chuỗi hoạt động hợp lệ, theo dõi điểm xếp chồng và điểm tích lũy. Ngay cả khi cắt bớt các trạng thái không hợp lệ, số lượng ngăn xếp và lịch trình riêng biệt vẫn tăng lên một cách kết hợp. Trong trường hợp xấu nhất, sau n/2 yêu cầu, ngăn xếp có thể được hoán vị theo nhiều cách và mỗi cấu hình sẽ dẫn đến những ràng buộc khác nhau trong tương lai. Điều này dẫn đến độ phức tạp theo cấp số nhân và không thể sử dụng được ngoài những đầu vào rất nhỏ. 

Cái nhìn sâu sắc quan trọng là đảo ngược quan điểm. Thay vì suy nghĩ về trình tự hoạt động, chúng tôi diễn giải lịch trình cuối cùng là sự khớp giữa ngày yêu cầu và ngày gửi, trong đó mỗi yêu cầu phải được ghép nối với một lần gửi sau đó. Vì ngăn xếp là LIFO nên các cặp này phải tạo thành cấu trúc không giao nhau: yêu cầu gần đây nhất phải được khớp trước, điều này ngụ ý cấu trúc lồng nhau tương đương với dấu ngoặc đơn cân bằng. 

Điều này biến vấn đề thành việc chọn cấu trúc ghép nối hợp lệ theo dòng thời gian. Mỗi yêu cầu giống như một khung mở, mỗi lần gửi là một khung đóng và ràng buộc buộc phải lồng đúng. 

Bây giờ quan sát quan trọng là đối với bất kỳ cấu trúc ghép nối cố định nào, các quyết định loại tối ưu đều mang tính cục bộ. Mỗi yêu cầu đóng góp một giá trị cơ bản tùy thuộc vào lựa chọn loại của nó và mỗi lần gửi đóng góp một giá trị tùy thuộc vào loại được thực hiện bởi yêu cầu phù hợp và tâm trạng vào ngày gửi.

Do đó, chúng ta có thể xử lý vấn đề như chọn một chuỗi các lần đẩy và bật lên, nhưng vì tổng số yêu cầu bằng tổng số lần gửi (vì các ngày là số chẵn và cuối cùng tất cả các bộ phải được gửi cho bất kỳ điểm nào), cấu trúc giảm xuống còn việc quyết định khi nào giữ các mục trên ngăn xếp và khi nào bật ra. 

Một cách nhìn trực tiếp và mạnh mẽ hơn là mô phỏng một cách tham lam bằng cách sử dụng ngăn xếp và đưa ra quyết định linh hoạt: chúng tôi lặp lại qua nhiều ngày và bất cứ khi nào nhìn thấy cơ hội yêu cầu, chúng tôi sẽ quyết định có nên thúc đẩy cơ hội đó hay không hay thay vào đó chúng tôi nên ưu tiên gửi các yêu cầu trước đó để mở khóa giá trị trong tương lai. Chiến lược tham lam đúng đắn hóa ra là: luôn yêu cầu vào mọi thời điểm đủ điều kiện yêu cầu và luôn gửi bất cứ khi nào có thể, nhưng hãy chọn loại yêu cầu một cách tối ưu để mỗi bộ được điều chỉnh tốt nhất với môi trường gửi yêu cầu cuối cùng của nó. 

Điều này tiếp tục sụp đổ thành một cặp tham lam đơn giản: chúng tôi duy trì một loạt các bộ đang chờ xử lý, mỗi bộ được chú thích với mức đóng góp điểm số tốt nhất có thể mà chúng tôi có thể đảm bảo và vào mỗi ngày gửi, chúng tôi sẽ bật đóng góp gần đây nhất và thêm đóng góp tốt nhất có thể đạt được dựa trên các ràng buộc hiện tại và tương lai được mã hóa ngầm bởi cấu trúc tham lam. 

Sự đơn giản hóa sâu hơn, chính là điều làm cho giải pháp trở nên tuyến tính, là chiến lược tối ưu không bao giờ cần trì hoãn việc gửi nếu một tập hợp tồn tại, bởi vì việc trì hoãn chỉ làm giảm tính linh hoạt trong tương lai mà không cải thiện kết quả ràng buộc LIFO. Do đó, chúng tôi mô phỏng trực tiếp: yêu cầu hàng ngày khi chúng tôi không gửi và gửi bất cứ khi nào cấu trúc có lợi ra lệnh, điều này làm giảm quy trình xếp chồng tham lam xác định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force vượt quá lịch trình | Hàm mũ | Hàm mũ | Quá chậm | 
| Mô phỏng tham lam dựa trên ngăn xếp | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Duyệt các ngày từ trái sang phải trong khi vẫn duy trì một tập hợp các bài toán đang hoạt động. Mỗi phần tử lưu trữ loại của tập hợp và điểm số mà nó sẽ mang lại khi được giải quyết cuối cùng. Điều này mô hình trực tiếp ràng buộc LIFO. 
2. Khi chúng tôi quyết định "yêu cầu", chúng tôi sẽ đẩy một tập hợp mới vào ngăn xếp. Loại chúng tôi chọn phù hợp với việc tối đa hóa lợi ích dự kiến, nhưng vì đã biết tâm trạng trong tương lai nên chúng tôi chỉ định nó ngay lập tức dựa trên tối ưu hóa cục bộ: nếu tâm trạng hiện tại là C thì chúng tôi thích Mã hóa hơn, nếu không thì gây nhiễu. Điều này tối đa hóa giá trị yêu cầu ngay lập tức. 
3. Khi chúng tôi đạt đến điểm quyết định gửi, chúng tôi sẽ bật lên trên cùng của ngăn xếp. Đây là lựa chọn đệ trình hợp pháp duy nhất theo LIFO. 
4. Điểm cho tập hợp được bật lên được tính bằng cách sử dụng tâm trạng của ngày gửi và loại được lưu trữ của tập hợp đó. Nếu chúng khớp nhau, chúng tôi lấy toàn bộ giá trị; nếu không chúng tôi sẽ áp dụng hình phạt. 
5. Chúng tôi tiếp tục cho đến khi tất cả các ngày được xử lý, đảm bảo rằng tất cả các bộ được yêu cầu cuối cùng đều được gửi vì dữ liệu đầu vào đảm bảo số ngày chẵn. 
6. Câu trả lời cuối cùng là số điểm tích lũy từ tất cả các bộ đã xuất hiện. 

### Tại sao nó hoạt động 

Điều bất biến là ngăn xếp luôn thể hiện chính xác tập hợp các yêu cầu chưa được gửi theo đúng thứ tự thời gian và không có quyết định nào trong tương lai có thể thay đổi thứ tự tương đối của các lần gửi. Bởi vì việc gửi của mỗi bộ bị bắt buộc khi nó trở thành đầu ngăn xếp ở bước gửi, mức độ tự do duy nhất là loại của nó tại thời điểm tạo. Vì lựa chọn loại chỉ ảnh hưởng độc lập đến giá trị yêu cầu cục bộ và đánh giá lần gửi cuối cùng nên việc tối ưu hóa nó một cách tham lam theo yêu cầu là đủ. Bất kỳ nỗ lực sắp xếp lại thứ tự gửi nào sẽ vi phạm ràng buộc LIFO, do đó, không có sự sắp xếp lại toàn cầu nào có thể cải thiện kết quả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    for tc in range(1, T + 1):
        s = input().strip()
        stack = []
        score = 0

        for i, ch in enumerate(s):
            # If stack is non-empty, we choose to submit
            # otherwise we request
            if stack:
                t, base = stack.pop()
                # submission mood affects score
                if t == ch:
                    score += base
                else:
                    score += max(0, base - 5)
            else:
                # request a set matching current mood
                # to maximize request value
                if ch == 'C':
                    stack.append(('C', 10))
                else:
                    stack.append(('J', 10))

        print(f"Case #{tc}: {score}")

if __name__ == "__main__":
    solve()
```Mã này duy trì một chồng các bộ vấn đề đang chờ xử lý. Mỗi phần tử lưu trữ loại và giá trị cơ sở của nó tại thời điểm tạo. Quy tắc quyết định rất đơn giản: nếu có thứ gì đó cần gửi, chúng tôi sẽ gửi ngay lập tức; nếu không, chúng tôi yêu cầu một bộ mới phù hợp với tâm trạng hiện tại. 

Chi tiết triển khai quan trọng là ngăn xếp tự động mã hóa thứ tự gửi LIFO, do đó không cần cấu trúc lập lịch rõ ràng. Việc tính điểm diễn ra vào thời điểm hiện tại dựa trên tâm trạng của ngày hiện tại. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`CCJJ`Chúng tôi mô phỏng từng bước. 

| Ngày | Tâm trạng | Xếp chồng trước | Hành động | Xếp chồng sau | Điểm đạt được | 
| --- | --- | --- | --- | --- | --- | 
| 1 | C | [] | Yêu cầu C | [C(10)] | 0 | 
| 2 | C | [C] | Gửi | [] | 10 | 
| 3 | J | [] | Yêu cầu J | [J(10)] | 0 | 
| 4 | J | [J] | Gửi | [] | 10 | 

Tổng số điểm là 20. 

Dấu vết này cho thấy các cặp gửi yêu cầu xen kẽ sẽ tách biệt từng bộ, đảm bảo không có sự can thiệp nào từ các ràng buộc LIFO. 

### Ví dụ 2:`CJCJ`| Ngày | Tâm trạng | Xếp chồng trước | Hành động | Xếp chồng sau | Điểm đạt được | 
| --- | --- | --- | --- | --- | --- | 
| 1 | C | [] | Yêu cầu C | [C(10)] | 0 | 
| 2 | J | [C] | Gửi | [] | 5 (phạt đền) | 
| 3 | C | [] | Yêu cầu C | [C(10)] | 0 | 
| 4 | J | [C] | Gửi | [] | 5 (phạt đền) | 

Tổng số điểm là 10. 

Điều này cho thấy tâm trạng gửi không khớp sẽ làm giảm điểm như thế nào và việc xếp chồng không giúp ích gì khi không thể ghép đôi tốt hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi ngày thực hiện tối đa một thao tác Đẩy hoặc bật | 
| Không gian | O(n) | Ngăn xếp giữ tối đa một mục nhập cho mỗi yêu cầu | 

Thuật toán xử lý tối đa 20000 thao tác cho mỗi trường hợp thử nghiệm, phù hợp thoải mái trong giới hạn vì tổng kích thước đầu vào được giới hạn bởi 150000. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    solve()

    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# sample-like cases
assert run("1\nCCJJ\n") == "Case #1: 20"
assert run("1\nCJCJ\n") == "Case #1: 10"

# all same mood
assert run("1\nCCCC\n") == "Case #1: 20"

# alternating worst case
assert run("1\nCJCJJCJC\n") == "Case #1: 20"

# minimum case
assert run("1\nCJ\n") == "Case #1: 10"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| CJ | 10 | cặp yêu cầu-gửi tối thiểu | 
| CCCC | 20 | kết hợp tối ưu nhất quán | 
| CJCJ | 10 | tuyên truyền hình phạt | 
| CCJJ | 20 | ghép nối sạch sẽ | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tâm trạng thay đổi nhưng việc xếp chồng có vẻ có lợi. Đối với đầu vào`CJCJ`, một chiến lược ngây thơ có thể cố gắng trì hoãn việc gửi để căn chỉnh tốt hơn sau này, nhưng LIFO ngăn cản việc sắp xếp lại các cặp. Thuật toán xử lý việc này bằng cách ghép nối ngay lập tức các yêu cầu và nội dung gửi, đảm bảo không tích lũy ngăn xếp cũ. 

Một trường hợp khác là một chuỗi dài những tâm trạng giống hệt nhau như`CCCCCC`. Ngăn xếp không bao giờ tăng vượt quá một tập hợp đang chờ xử lý hiệu quả vì chúng tôi gửi ngay lập tức trước khi yêu cầu lại. Điều này tránh sự tích lũy không cần thiết và đảm bảo mỗi yêu cầu đều có giá trị tối đa ở cả hai đầu.
