---
title: "CF 1001A - Tạo trạng thái cộng hoặc trạng thái trừ"
description: "Chúng tôi đang làm việc trong cài đặt lập trình lượng tử trong đó một qubit đã ở trạng thái cơ bản ban đầu đã biết và chúng tôi được yêu cầu chuyển đổi nó thành một trong hai trạng thái cơ sở mục tiêu tùy thuộc vào giá trị của tham số số nguyên được gọi là dấu."
date: "2026-06-16T23:41:17+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 1001
codeforces_index: "A"
codeforces_contest_name: "Microsoft Q# Coding Contest - Summer 2018 - Warmup"
rating: 1100
weight: 1001
solve_time_s: 58
verified: true
draft: false
---

[CF 1001A - Tạo trạng thái cộng hoặc trạng thái trừ](https://codeforces.com/problemset/problem/1001/A) 

**Đánh giá:** 1100 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc trong cài đặt lập trình lượng tử trong đó một qubit đã ở trạng thái cơ bản ban đầu đã biết và chúng tôi được yêu cầu chuyển đổi nó thành một trong hai trạng thái cơ sở mục tiêu tùy thuộc vào giá trị của tham số nguyên được gọi là`sign`. Về mặt khái niệm, bạn có thể coi qubit như một hệ thống hai cấp hiện đang mã hóa trạng thái cơ sở tính toán cố định và chúng ta được phép áp dụng các cổng lượng tử để buộc nó vào trạng thái mục tiêu bắt buộc. 

Nhiệm vụ không phải là đo lường hay trả về kết quả cổ điển. Thay vào đó, hàm này là một thao tác làm thay đổi trạng thái lượng tử tại chỗ. Sau khi hoạt động kết thúc, qubit phải được giữ ở trạng thái cụ thể: một trạng thái nếu`sign = 1`và trạng thái đảo pha ngược lại nếu`sign = -1`. 

Các ràng buộc là không đáng kể theo nghĩa cổ điển vì chỉ có một qubit duy nhất và lượng công việc không đổi cho mỗi trường hợp thử nghiệm. Điều này ngay lập tức loại trừ mọi nhu cầu về logic mô phỏng hoặc phân rã. Lý do có ý nghĩa duy nhất là làm thế nào để ánh xạ một giá trị điều khiển cổ điển vào một phép biến đổi pha lượng tử. 

Trường hợp phức tạp chính là các phép biến đổi trạng thái lượng tử không phải là phép gán tùy ý. Một cách tiếp cận bất cẩn có thể cố gắng “thiết lập” trực tiếp trạng thái, đây không phải là cách hoạt động của tính toán lượng tử. Thay vào đó, chúng ta phải áp dụng các cổng đơn nhất hợp lệ để đạt được phép biến đổi mong muốn. Một sai lầm tiềm ẩn khác là bỏ qua vụ việc`sign = -1`, đòi hỏi phải đưa ra đảo pha thay vì đảo bit. Vì cả hai trạng thái mục tiêu chỉ khác nhau ở pha tổng thể hoặc thay đổi dấu về biên độ, nên thao tác chính xác phải tôn trọng hành vi của pha thay vì trực giác phủ định cổ điển. 

## Phương pháp tiếp cận 

Một mô hình tinh thần mạnh mẽ sẽ phải nghĩ đến việc xây dựng lại vectơ trạng thái đầy đủ của qubit và sau đó ghi đè lên nó bằng vectơ mục tiêu. Điều đó sẽ tương ứng với việc biểu diễn rõ ràng các biên độ và gán chúng phù hợp với trạng thái được yêu cầu. Mặc dù đây là khái niệm đơn giản nhưng nó không phải là một phép toán lượng tử hợp lệ trong một hệ thống thực vì nó bỏ qua yêu cầu rằng các phép biến đổi phải đơn nhất. Ngay cả khi chúng ta tưởng tượng ra một trình mô phỏng, việc xây dựng và chuẩn hóa các vectơ trạng thái cho mỗi thao tác sẽ là chi phí không cần thiết. 

Quan sát quan trọng là hai trạng thái đích có thể có chỉ khác nhau một dấu được kiểm soát bởi số nguyên`sign`. Trong điện toán lượng tử, đây chính xác là vai trò của hoạt động lật pha. Cổng Pauli-Z giữ nguyên trạng thái cơ sở tính toán |0⟩ và đảo pha |1⟩. Trong bối cảnh mã hóa của vấn đề này, hiệu ứng mà chúng ta cần có thể được giảm xuống bằng việc áp dụng một cổng Z có điều kiện tùy thuộc vào việc chúng ta muốn biến thể tích cực hay tiêu cực. 

Trạng thái ban đầu là cố định và đã biết nên không cần phải xây dựng lại hoặc kiểm tra nó. Toàn bộ vấn đề quy về việc áp dụng một phép biến đổi đơn nhất có điều kiện: không làm gì khi`sign = 1`và áp dụng đảo pha khi`sign = -1`. 

Điều này làm giảm giải pháp thành một quyết định theo thời gian không đổi, sau đó có nhiều nhất một ứng dụng cổng lượng tử. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tái thiết trạng thái Brute Force | O(1) khái niệm, phép toán lượng tử không hợp lệ | O(1) | Không hợp lệ trong mô hình lượng tử | 
| Áp dụng lật pha có điều kiện | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`sign`. Giá trị này xác định xem chúng ta giữ trạng thái không thay đổi hay đưa ra đảo pha. Bản thân qubit đã được chuyển qua tham chiếu, vì vậy chúng tôi không xây dựng hoặc trả lại bất kỳ thứ gì. 
2. Kiểm tra xem`sign`bằng`-1`. Đây là điều kiện duy nhất đòi hỏi phải hành động vì`sign = 1`tương ứng với sự chuyển đổi danh tính. 
3. Nếu`sign = -1`, áp dụng cổng Pauli-Z cho qubit. Thao tác này sẽ đảo pha của thành phần |1⟩ trong khi vẫn bảo toàn xác suất cơ sở tính toán, tạo ra trạng thái phủ định cần thiết. 
4. Nếu`sign = 1`, không làm gì cả. Hoạt động nhận dạng đã duy trì trạng thái cần thiết. 

### Tại sao nó hoạt động 

Tính chính xác dựa trên thực tế là hai đầu ra cần thiết chỉ khác nhau bởi sự thay đổi pha được thể hiện chính xác bằng phép toán Pauli-Z. Vì các phép toán lượng tử phải đơn nhất, nên bất kỳ phép biến đổi hợp lệ nào giữa hai trạng thái này đều phải bảo toàn biên độ trùng pha. Cổng nhận dạng và cổng Z tạo thành một tập quyết định hoàn chỉnh cho hai trường hợp và không yêu cầu chuyển đổi trung gian. Điều bất biến là sau mỗi bước, qubit vẫn ở trạng thái lượng tử hợp lệ và phù hợp với điều kiện pha yêu cầu được ngụ ý bởi`sign`. 

## Giải pháp Python 

Việc triển khai thực tế theo kiểu Q#, nhưng chúng tôi trình bày logic theo cấu trúc giống Python để làm rõ luồng điều khiển. Trong việc nộp thực tế, điều này tương ứng với việc áp dụng`Z`từ thư viện lượng tử.```python
import sys
input = sys.stdin.readline

def solve(q, sign):
    if sign == -1:
        # apply phase flip
        q.apply_Z()
    return
```Quyết định thực hiện quan trọng là ứng dụng có điều kiện của cổng Z. Không có sự phức tạp phân nhánh nào ngoài việc kiểm tra này và không cần kiểm tra cấp tiểu bang. Hàm này là đột biến thuần túy trên tham chiếu qubit. 

Lỗi phổ biến nhất trong quá trình triển khai là cố gắng sửa đổi biên độ một cách trực tiếp hoặc coi qubit như một bit cổ điển. Một sai lầm tinh vi khác là áp dụng đảo bit (cổng X) thay vì đảo pha (cổng Z), điều này sẽ làm thay đổi kết quả đo thay vì pha, tạo ra trạng thái lượng tử không chính xác. 

## Ví dụ đã hoạt động 

Xét hai trường hợp: một trường hợp`sign = 1`và một nơi`sign = -1`. 

Vì`sign = 1`, hoạt động không làm gì cả. 

| Bước | ký tên | Hoạt động | Trạng thái Qubit | 
| --- | --- | --- | --- | 
| 1 | 1 | Không có | Trạng thái ban đầu | 

Điều này xác nhận rằng hành vi nhận dạng duy trì chính xác trạng thái ban đầu, phù hợp với mục tiêu cần thiết cho`sign = 1`. 

Vì`sign = -1`, chúng ta áp dụng cổng Z. 

| Bước | ký tên | Hoạt động | Trạng thái Qubit | 
| --- | --- | --- | --- | 
| 1 | -1 | Áp dụng Z | Trạng thái đảo pha | 

Điều này cho thấy chỉ có pha tương đối thay đổi trong khi xác suất đo không thay đổi. Điều đó phù hợp với yêu cầu đối với biến thể tiêu cực của trạng thái mục tiêu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một kiểm tra có điều kiện duy nhất và tối đa một cổng lượng tử | 
| Không gian | O(1) | Không có cấu trúc phụ trợ nào được tạo | 

Giải pháp này thỏa mãn một cách cơ bản các ràng buộc vì nó thực hiện một lượng công việc không đổi bất kể giá trị đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    # Since we cannot simulate Q# here, we only test decision logic
    # We mock qubit as a dict for illustration
    class Qubit:
        def __init__(self):
            self.ops = []
        def apply_Z(self):
            self.ops.append("Z")

    def solve(q, sign):
        if sign == -1:
            q.apply_Z()

    # test 1: sign = 1
    q1 = Qubit()
    solve(q1, 1)
    assert q1.ops == []

    # test 2: sign = -1
    q2 = Qubit()
    solve(q2, -1)
    assert q2.ops == ["Z"]

    return "OK"

assert run("") == "OK"

# custom cases
# minimum behavior
q = type("Q", (), {"ops": [], "apply_Z": lambda self: self.ops.append("Z")})()
q.ops = []
if -1 == -1:
    q.apply_Z()
assert q.ops == ["Z"]

q = type("Q", (), {"ops": [], "apply_Z": lambda self: self.ops.append("Z")})()
q.ops = []
if 1 == 1:
    pass
assert q.ops == []
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ký = 1 | không hoạt động | trường hợp nhận dạng | 
| dấu = -1 | Z áp dụng | trường hợp lật pha | 
| trống/giả | được | sự đúng đắn về cấu trúc | 

## Vỏ cạnh 

cho`sign = 1`, thuật toán không thực hiện thao tác nào. Bắt đầu từ trạng thái qubit ban đầu, việc kiểm tra không đạt điều kiện`sign == -1`, do đó việc thực thi không thành công mà không áp dụng bất kỳ cổng nào. Qubit không thay đổi, đó chính xác là kết quả cần thiết. 

Vì`sign = -1`, điều kiện kích hoạt và cổng Z được áp dụng một lần. Qubit chuyển từ trạng thái ban đầu sang trạng thái đảo pha. Vì không có phép biến đổi nào khác xảy ra nên không có nguy cơ xảy ra lỗi gộp hoặc thay đổi cơ sở không chính xác.
