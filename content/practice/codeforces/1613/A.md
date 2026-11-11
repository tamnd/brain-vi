---
title: "CF 1613A - So sánh dài"
description: "Mỗi số trong bài toán này không được đưa ra ở dạng mở rộng mà dưới dạng mô tả ngắn gọn. Thay vì viết số nguyên đầy đủ, chúng ta được cung cấp một giá trị cơ bản x và một số số 0 ở cuối p, nghĩa là số thực tế là x × 10^p."
date: "2026-06-10T06:53:37+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1613
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 118 (Rated for Div. 2)"
rating: 900
weight: 1613
solve_time_s: 79
verified: true
draft: false
---

[CF 1613A - So sánh dài](https://codeforces.com/problemset/problem/1613/A) 

**Xếp hạng:** 900 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi số trong bài toán này không được đưa ra ở dạng mở rộng mà dưới dạng mô tả ngắn gọn. Thay vì viết số nguyên đầy đủ, chúng ta được cung cấp một giá trị cơ bản`x`và một số số 0 ở cuối`p`, nghĩa là số thực tế là`x × 10^p`. 

Vì vậy, mọi trường hợp thử nghiệm đều đưa ra hai số “chuyển số thập phân” như vậy. Nhiệm vụ chỉ đơn giản là xác định số nguyên nào trong hai số nguyên thu được lớn hơn hoặc liệu chúng có bằng nhau hay không. 

Khó khăn chính đó là`p`có thể lớn tới 10^6. Điều đó khiến cho việc xây dựng số thực tế là không thể, vì việc viết ra 10^6 chữ số đã vượt xa giới hạn về bộ nhớ và thời gian. Việc so sánh phải được thực hiện hoàn toàn từ cấu trúc của các con số. 

Các ràng buộc cho phép tối đa 10^4 trường hợp thử nghiệm. Bất kỳ giải pháp nào cố gắng xây dựng hoặc mô phỏng rõ ràng các số đầy đủ sẽ ngay lập tức thất bại do lỗi cả về thời gian và bộ nhớ. Ngay cả việc xây dựng dựa trên chuỗi cũng không khả thi vì một trường hợp thử nghiệm có thể yêu cầu tới một triệu chữ số. 

Trường hợp cạnh tinh tế xuất hiện khi số cơ sở nhỏ nhưng số mũ khác nhau. Ví dụ, so sánh`1 × 10^5`Và`10 × 10^4`hình như đang so sánh`100000`Và`100000`, bằng nhau, mặc dù các giá trị cơ bản khác nhau. Một cách tiếp cận ngây thơ so sánh`(x, p)`về mặt từ điển sẽ kết luận không chính xác rằng`10 × 10^4`lớn hơn bởi vì`10 > 1`. 

Một trường hợp sai sót khác phát sinh khi số mũ của một số lấn át hoàn toàn độ lớn của số kia, chẳng hạn`999 × 10^0`so với`1 × 10^3`. Câu trả lời đúng hoàn toàn phụ thuộc vào độ dài chữ số sau khi khai triển chứ không phụ thuộc vào độ lớn cơ sở. 

Do đó, việc so sánh phải mô phỏng độ lớn số học mà không cần xây dựng các con số. 

## Phương pháp tiếp cận 

Phương pháp brute-force trực tiếp sẽ xây dựng biểu diễn số nguyên đầy đủ của mỗi số bằng cách nối thêm`p`số không đến chuỗi thập phân của`x`, sau đó so sánh các chuỗi kết quả bằng số. Về mặt khái niệm, điều này đơn giản và chính xác vì nó phản ánh những con số thực tế. 

Tuy nhiên, trong trường hợp xấu nhất, mỗi số có thể có tới 10^6 chữ số. Với tối đa 10^4 trường hợp thử nghiệm, điều này dẫn đến kích thước đầu ra theo lý thuyết ở mức 10^10 ký tự. Ngay cả khi bỏ qua giới hạn bộ nhớ, chỉ cần viết hoặc so sánh các chuỗi này sẽ vượt xa giới hạn về thời gian. 

Quan sát quan trọng là các số 0 ở cuối chỉ ảnh hưởng đến thang đo của số chứ không ảnh hưởng đến cấu trúc chữ số bên trong của nó. Sự so sánh có ý nghĩa duy nhất đến từ tổng chiều dài chữ số sau khi mở rộng. Nếu hai số có độ dài hiệu dụng khác nhau thì số nào dài hơn sẽ lớn hơn ngay lập tức. Nếu chúng có cùng độ dài, việc so sánh sẽ giảm xuống còn so sánh các tiền tố có nghĩa của chúng với từng chữ số, vì các số 0 ở cuối căn chỉnh hoàn hảo. 

Điều này làm giảm vấn đề cân bằng độ dài chữ số và sau đó so sánh các biểu diễn chuẩn hóa của các số có độ dài bằng nhau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (xây dựng số lượng đầy đủ) | O(tổng của 10^p chữ số) | O(tổng của 10^p) | Quá chậm | 
| Tối ưu (độ dài + chuẩn hóa) | O(t · log x) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta coi mỗi số có hai thành phần: một phần quan trọng`x`và sự thay đổi độ lớn`p`. 

1. Tính độ dài hiệu dụng của mỗi số như sau`len(str(x)) + p`. 

Điều này biểu thị số lượng mở rộng đầy đủ sẽ có bao nhiêu chữ số nếu không tạo ra nó. 
2. Nếu hai độ dài khác nhau thì số có độ dài lớn hơn sẽ tự động lớn hơn. 

Điều này xuất phát trực tiếp từ thực tế là việc thêm nhiều số 0 ở cuối luôn làm tăng độ lớn lên gấp mười. 
3. Nếu độ dài bằng nhau, chúng ta không thể quyết định chỉ dựa vào độ dài. Trong trường hợp này, chúng ta so sánh từng chữ số như thể cả hai số đã được mở rộng hoàn toàn. 
4. Để so sánh mà không cần mở rộng, chúng tôi căn chỉnh cả hai số về mặt khái niệm: 

số mũ lớn hơn đóng góp thêm các số 0 ở cuối vào cơ số nhỏ hơn, vì vậy chúng ta so sánh một cách hiệu quả`x1 × 10^(p1 - p2)`Và`x2 × 10^(p2 - p1)`tùy theo số mũ nào lớn hơn. 
5. Chúng tôi mô phỏng điều này bằng cách đệm biểu diễn nhỏ hơn một cách hợp lý bằng các số 0 và chỉ so sánh các phần quan trọng chồng chéo. 
6. Việc so sánh được thực hiện dưới dạng so sánh chuỗi của các biểu diễn căn chỉnh được xây dựng nhưng không thực sự hiện thực hóa các chuỗi đầy đủ. 

### Tại sao nó hoạt động 

Mọi số đều có dạng`x × 10^p`, tương đương với việc dịch chuyển biểu diễn thập phân của`x`bên trái bởi`p`các vị trí. Độ lớn của một số trong cơ số 10 trước tiên được xác định bằng số chữ số và chỉ sau đó bằng cách so sánh từ điển các chữ số khi độ dài khớp nhau. 

Bằng cách tách từng số thành “lớp độ dài” và “chuỗi chữ số căn chỉnh”, chúng tôi bảo toàn thứ tự chính xác của các số nguyên thực. Không có phép biến đổi nào làm thay đổi thứ tự tương đối vì phép nhân với lũy thừa 10 chỉ ảnh hưởng đến vị trí chữ số chứ không ảnh hưởng đến giá trị chữ số. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
out = []

for _ in range(t):
    x1, p1 = map(int, input().split())
    x2, p2 = map(int, input().split())

    s1 = str(x1)
    s2 = str(x2)

    len1 = len(s1) + p1
    len2 = len(s2) + p2

    if len1 > len2:
        out.append(">")
        continue
    if len1 < len2:
        out.append("<")
        continue

    # same length case: compare digit by digit
    i = 0
    j = 0
    zero1 = p1
    zero2 = p2

    while i < len(s1) or j < len(s2):
        d1 = int(s1[i]) if i < len(s1) else 0
        d2 = int(s2[j]) if j < len(s2) else 0

        if d1 != d2:
            out.append(">" if d1 > d2 else "<")
            break

        i += 1
        j += 1
    else:
        out.append("=")

sys.stdout.write("\n".join(out))
```Việc triển khai bắt đầu bằng cách chuyển đổi từng số cơ sở thành một chuỗi để việc truy cập chữ số có hiệu quả. Đầu tiên, nó so sánh tổng độ dài hiệu dụng để giải quyết hầu hết các trường hợp ngay lập tức. 

Khi độ dài khớp nhau, mã sẽ thực hiện so sánh chữ số được đồng bộ hóa. Khi một chuỗi hết chữ số, nó sẽ ngầm coi các vị trí bị thiếu là 0, tương ứng chính xác với tác động của các số 0 ở cuối trong số ban đầu. 

Một điểm tinh tế là chúng ta không bao giờ xây dựng các chuỗi đệm một cách rõ ràng. Thay vào đó, chúng tôi mô phỏng khoảng đệm bằng cách coi các vị trí ngoài phạm vi là số 0 trong quá trình so sánh. Điều này tránh mọi nguy cơ tràn bộ nhớ trong khi vẫn đảm bảo tính chính xác. 

## Ví dụ đã hoạt động 

Hãy xem xét cặp mẫu`(2, 1)`Và`(19, 0)`. 

Chúng tôi so sánh độ dài hiệu quả:`2 × 10^1`có độ dài 2, trong khi`19 × 10^0`cũng có độ dài bằng 2. Vì độ dài bằng nhau nên chúng ta so sánh từng chữ số. 

| Bước | s1 chữ số | chữ số s2 | Quyết định | 
| --- | --- | --- | --- | 
| 1 | 2 | 1 | 2 > 1 → đầu tiên lớn hơn | 

Vì vậy đầu ra là`>`. 

Bây giờ hãy xem xét`(10, 2)`Và`(100, 1)`. 

Độ dài hiệu dụng: cả hai đều có 3 chữ số. 

| Bước | s1 chữ số | chữ số s2 | Quyết định | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | bằng | 
| 2 | 0 | 0 | bằng | 
| 3 | 0 | 0 | bằng | 

Tất cả các chữ số đều khớp nhau, vì vậy kết quả là`=`. 

Các ví dụ này cho thấy thuật toán trước tiên sử dụng ưu thế về cấu trúc (độ dài) và chỉ quay lại căn chỉnh chữ số khi cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t · log x) | Mỗi bài kiểm tra xử lý các chuỗi chữ số có kích thước lên tới 6 | 
| Không gian | O(1) | Chỉ có không gian bổ sung liên tục ngoài bộ nhớ đầu ra | 

Thuật toán này hiệu quả vì nó tránh được mọi sự phụ thuộc vào`p`, có thể cực kỳ lớn. Tất cả các hoạt động được giới hạn bởi số chữ số trong`x`, nó nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        x1, p1 = map(int, input().split())
        x2, p2 = map(int, input().split())

        s1 = str(x1)
        s2 = str(x2)

        len1 = len(s1) + p1
        len2 = len(s2) + p2

        if len1 > len2:
            out.append(">")
        elif len1 < len2:
            out.append("<")
        else:
            i = j = 0
            while i < len(s1) or j < len(s2):
                d1 = int(s1[i]) if i < len(s1) else 0
                d2 = int(s2[j]) if j < len(s2) else 0
                if d1 != d2:
                    out.append(">" if d1 > d2 else "<")
                    break
                i += 1
                j += 1
            else:
                out.append("=")

    return "\n".join(out)

# provided sample
assert run("""5
2 1
19 0
10 2
100 1
1999 0
2 3
1 0
1 0
99 0
1 2
""") == """>\n=\n<\n=\n<"""

# edge: equal expansion
assert run("""1
1 3
1000 0
""") == "="

# edge: exponent dominates
assert run("""1
1 5
999 0
""") == ">"""

# edge: same x different p
assert run("""1
10 1
10 2
""") == "<"""

# edge: large equal structure
assert run("""1
123 2
12300 0
""") == "="
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 3 vs 1000 0`|`=`| Độ lớn được cân bằng thông qua các số 0 ở cuối | 
|`1 5 vs 999 0`|`>`| Sự thống trị về độ dài từ số mũ | 
|`10 1 vs 10 2`|`<`| Tác dụng của số 0 ở cuối | 
|`123 2 vs 12300 0`|`=`| Tương đương cấu trúc chính xác | 

## Vỏ cạnh 

Trường hợp một cạnh là khi một số trở nên dài hơn đáng kể do có các số 0 ở cuối, mặc dù cơ số của nó nhỏ. Đối với đầu vào`1 5`Và`999 0`, số đầu tiên trở thành 100000 trong khi số thứ hai vẫn là 999. Thuật toán ngay lập tức so sánh độ dài, tìm ra số đầu tiên dài hơn và đưa ra kết quả`>`mà không kiểm tra chữ số. 

Một trường hợp khác xảy ra khi cả hai số đều có độ dài hiệu dụng giống hệt nhau nhưng yêu cầu căn chỉnh chữ số. Vì`10 2`Và`100 1`, cả hai đều mở rộng đến ba chữ số. Thuật toán so sánh từng chữ số và tìm sự bằng nhau ở mọi vị trí, xuất ra chính xác`=`. 

Trường hợp thứ ba liên quan đến các giá trị cơ sở giống hệt nhau với số mũ khác nhau, chẳng hạn như`10 1`Và`10 2`. Độ dài hiệu dụng khác nhau, do đó việc so sánh được giải quyết ngay lập tức mà không cần kiểm tra chữ số, phản ánh chính xác rằng việc thêm số 0 ở cuối sẽ làm tăng độ lớn.
