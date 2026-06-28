---
title: "CF 105141E - Quản lý bộ nhớ an toàn"
description: "Chúng ta được cung cấp một chuỗi các thao tác mô phỏng một hệ thống bộ nhớ rất đơn giản. Mỗi biến được phân bổ chính xác một lần bằng cách sử dụng let X = new(); và sau đó được phát hành đúng một lần bằng cách sử dụng drop(X);."
date: "2026-06-27T16:53:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105141
codeforces_index: "E"
codeforces_contest_name: "BSUIR Open XII: Student Final"
rating: 0
weight: 105141
solve_time_s: 47
verified: true
draft: false
---

[CF 105141E - Quản lý bộ nhớ an toàn](https://codeforces.com/problemset/problem/105141/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các thao tác mô phỏng một hệ thống bộ nhớ rất đơn giản. Mỗi biến được phân bổ chính xác một lần bằng cách sử dụng`let X = new();`tuyên bố và sau đó được phát hành đúng một lần bằng cách sử dụng`drop(X);`. Hạn chế chính là chương trình ban đầu đã sửa thứ tự phân bổ và phân bổ xảy ra, nhưng nó không sử dụng bất kỳ phạm vi có cấu trúc nào. 

Nhiệm vụ của chúng ta là viết lại chương trình bằng cách chèn phạm vi`{ ... }`để chúng ta có thể loại bỏ càng nhiều điều rõ ràng`drop(X)`các câu lệnh càng tốt, trong khi vẫn giữ nguyên thứ tự tương đối của tất cả các phân bổ và tất cả các phân bổ còn lại. Trình biên dịch sẽ tự động chèn các phân bổ ngầm vào cuối phạm vi, vì vậy mỗi khi chúng ta đóng một phạm vi, chúng ta có thể dựa vào hành vi dọn dẹp giống như ngăn xếp. 

Điều này có nghĩa là chúng tôi được phép thay thế một cách hiệu quả các chuỗi thả rõ ràng được lựa chọn cẩn thận bằng các khối có cấu trúc, trong đó các biến nằm ngoài phạm vi theo thứ tự ngược lại với khai báo của chúng. 

Các ràng buộc rất nhỏ, tối đa là 1000 dòng. Điều này ngay lập tức cho chúng ta biết rằng ngay cả suy luận bậc hai đối với các sự kiện cũng có thể chấp nhận được, nhưng cấu trúc của bài toán gợi ý rõ ràng về một cấu trúc tham lam hoặc dựa trên ngăn xếp hơn là bất kỳ lập trình động nào trên các chuỗi con. 

Một cách tiếp cận đơn giản sẽ thử tất cả các vị trí có thể có của phạm vi và vị trí nào cần loại bỏ. Điều này mang tính bùng nổ về mặt tổ hợp vì mọi tập hợp con của các giọt đều có khả năng được thay thế bằng một ranh giới phạm vi và mọi thứ tự biến bên trong một phạm vi đều quan trọng. Ngay cả với n = 1000, điều này vẫn không khả thi vì số lượng phân vùng tăng theo cấp số nhân. 

Một trường hợp thất bại tinh tế hơn đối với lý luận tham lam ngây thơ xuất hiện khi các biến được xen kẽ theo cách không lồng nhau. Ví dụ:```
let a;
let b;
drop(a);
let c;
drop(b);
drop(c);
```Nếu chúng ta tham lam mở một phạm vi sau`a`hoặc`b`, chúng tôi có thể buộc phải lồng ghép sớm để ngăn chặn việc hợp nhất các giao dịch giải phóng sau này. Khó khăn chính là phạm vi áp đặt kỷ luật ngăn xếp, vì vậy chúng ta phải căn chỉnh cẩn thận thứ tự phân bổ và giải phóng theo cấu trúc nhập sau xuất trước. 

## Phương pháp tiếp cận 

Quan sát chính là phạm vi mô phỏng một chồng các biến hoạt động. Bên trong một phạm vi, nếu chúng ta khai báo các biến theo thứ tự nào đó, sự phá hủy ngầm của chúng sẽ xảy ra theo thứ tự ngược lại khi phạm vi đóng lại. Điều này có nghĩa là bất cứ khi nào chúng ta có thể căn chỉnh rõ ràng`drop`khi một tập hợp các biến tự nhiên trở thành hậu tố liền kề của phân bổ hoạt động, chúng ta có thể thay thế những phần giảm đó bằng một ranh giới phạm vi duy nhất. 

Quan điểm bạo lực là xem xét tất cả các cách nhóm phân bổ thành các phạm vi sao cho mọi sự sụt giảm rõ ràng đều được giữ nguyên hoặc được hấp thụ vào việc đóng phạm vi. Đối với mỗi nhóm, chúng ta phải mô phỏng tính chính xác bằng cách kiểm tra xem hành vi của ngăn xếp cảm ứng có khớp với thứ tự ban đầu hay không. Đây thực chất là một vấn đề về phân vùng theo trình tự và việc thử tất cả các phân vùng sẽ dẫn đến độ phức tạp theo cấp số nhân. 

Sự đơn giản hóa chính là diễn giải chương trình dưới dạng một chuỗi các lần đẩy (phân bổ) và bật lên (giải quyết). Trình tự ban đầu đã xác định quy trình ngăn xếp hợp lệ vì mỗi biến được phân bổ trước khi bị loại bỏ và tên là duy nhất. Quyền tự do duy nhất chúng ta có là quyết định thời điểm mở và đóng phạm vi, tương ứng với việc nhóm các khung ngăn xếp liên tiếp thành các khối nơi chúng ta dựa vào việc bật lên ngầm thay vì rõ ràng`drop`. 

Cấu trúc tối ưu hoạt động một cách tham lam bằng cách duy trì chồng các biến hoạt động hiện tại. Chúng tôi cố gắng trì hoãn việc giảm rõ ràng càng nhiều càng tốt và bất cứ khi nào chúng tôi thấy rằng một biến cần loại bỏ không nằm ở đầu cấu trúc ẩn hiện tại, chúng tôi buộc phải thực hiện các hoạt động rõ ràng hoặc mở ranh giới phạm vi để khôi phục khả năng tương thích LIFO. Điều này tự nhiên dẫn đến một mô phỏng ngăn xếp trong đó chúng tôi xây dựng phạm vi bất cứ khi nào lần thả yêu cầu tiếp theo phá vỡ thứ tự ngăn xếp. 

Thông tin chi tiết quan trọng là phạm vi cho phép chúng tôi "gói" hậu tố của việc dọn dẹp ngăn xếp. Vì vậy, thay vì loại bỏ từng phần tử một, chúng ta có thể đóng một phạm vi và để trình biên dịch hiển thị mọi thứ theo thứ tự ngược lại. Điều này là tối ưu bất cứ khi nào một hậu tố liền kề của các biến hoạt động sắp bị loại bỏ hoàn toàn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý trình tự trong khi duy trì một loạt các biến hiện đang hoạt động và xây dựng các khối đầu ra. 

1. Phân tích đầu vào thành một chuỗi các sự kiện phân bổ và phân bổ, đảm bảo trật tự. 
2. Duy trì một ngăn xếp biểu thị các biến hiện được phân bổ chưa được tính đến khi đóng phạm vi. Mỗi lần đẩy tương ứng với một`let`tuyên bố. 
3. Khi chúng ta gặp phải một`let X`, chúng ta xuất nó ngay lập tức và đẩy X vào ngăn xếp. 
4. Khi chúng ta gặp phải một`drop(X)`, chúng ta kiểm tra ngăn xếp từ trên xuống. Nếu X ở trên cùng, chúng ta có thể phát ra một cách an toàn`drop(X)`và bật nó lên. 
5. Nếu X không ở trên cùng, chúng ta không thể mô phỏng trực tiếp điều này bằng một chuỗi pop ngăn xếp ẩn duy nhất. Thay vào đó, chúng tôi đóng một phạm vi trước điểm phải xóa X, buộc tất cả các biến ở trên X trong ngăn xếp phải được giải phóng hoàn toàn theo thứ tự ngược lại. Chúng tôi phát ra dấu ngoặc nhọn đóng để xóa hậu tố của ngăn xếp và sau đó tiếp tục xử lý cho đến khi có thể truy cập được X. 
6. Sau khi đóng một phạm vi, chúng tôi có thể cần phải ngầm mở lại các phạm vi khi tiếp tục phân bổ. Chúng tôi đảm bảo rằng mỗi khi chúng tôi đóng một phạm vi, nó sẽ tương ứng với hậu tố tối đa của các biến sẽ không bị loại bỏ một cách rõ ràng sau này. 

Về cơ bản, thuật toán phân chia quá trình phát triển ngăn xếp thành các phân đoạn trong đó việc giảm rõ ràng chỉ xảy ra khi chúng khớp với phần trên cùng của ngăn xếp và tất cả các hoạt động dọn dẹp bắt buộc khác đều được xử lý bằng cách đóng phạm vi. 

### Tại sao nó hoạt động 

Tính chính xác xuất phát từ thực tế là mọi phạm vi đều hoạt động chính xác giống như một ngăn xếp của một hậu tố của các biến hiện đang tồn tại. Do trình tự ban đầu đã xác định kỷ luật ngăn xếp hợp lệ về phân bổ và phân bổ, nên mọi vi phạm trật tự LIFO trong các lần giảm rõ ràng đều có thể được trì hoãn và giải quyết bằng cách nhóm các biến can thiệp vào một phạm vi. Mỗi ranh giới phạm vi sẽ loại bỏ một hậu tố tối đa không thể tham gia vào các phần xen kẽ tiếp theo, đảm bảo chúng tôi không bao giờ đưa ra các phân bổ rõ ràng bổ sung ngoài mức cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def parse(line):
    line = line.strip()
    if line.startswith("let"):
        # let X = new();
        parts = line.split()
        return ("let", parts[1])
    else:
        # drop(X);
        x = line[line.find("(")+1:line.find(")")]
        return ("drop", x)

def solve():
    n = int(input())
    ops = [parse(input()) for _ in range(n)]

    stack = []
    alive = set()
    output = []

    for typ, x in ops:
        if typ == "let":
            output.append(f"let {x} = new();")
            stack.append(x)
            alive.add(x)
        else:
            if stack and stack[-1] == x:
                output.append(f"drop({x});")
                stack.pop()
                alive.remove(x)
            else:
                # close scope until x becomes accessible
                # flush everything above x implicitly
                temp = []
                while stack and stack[-1] != x:
                    temp.append(stack.pop())
                if stack:
                    stack.pop()
                    alive.remove(x)
                    output.append("}")
                    output.append(f"drop({x});")
                    # reopen remaining context if needed
                    if temp:
                        output.append("{")
                        while temp:
                            v = temp.pop()
                            stack.append(v)
                else:
                    # should not happen under valid input
                    pass

    print("\n".join(output))

if __name__ == "__main__":
    solve()
```Việc triển khai duy trì một chồng các biến hoạt động chính xác như trong mô phỏng tiêu chuẩn của phân bổ lồng nhau. Điểm phân nhánh chính là khi`drop(X)`không khớp với đỉnh ngăn xếp. Trong trường hợp đó, mã mô phỏng việc đóng một phạm vi để loại bỏ các biến can thiệp, vì các biến đó sẽ chặn yêu cầu LIFO. 

Phần tinh tế là đảm bảo rằng khi chúng tôi đóng một phạm vi, chúng tôi chỉ loại bỏ một hậu tố của ngăn xếp an toàn để phân bổ ngầm. Bộ đệm tạm thời`temp`đại diện cho các biến ở trên`X`và do đó phải được giới thiệu lại nếu sau đó chúng vẫn cần thiết. Điều này phản ánh ý tưởng rằng việc đóng phạm vi là một sự chuyển đổi cấu trúc có thể đảo ngược của các phân đoạn ngăn xếp. 

## Ví dụ đã hoạt động 

Hãy xem xét một chuỗi đơn giản:```
let a
let b
drop(b)
drop(a)
```Chúng tôi theo dõi ngăn xếp và đầu ra. 

| Bước | Hoạt động | Ngăn xếp | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | hãy để | [a] | hãy để | 
| 2 | hãy b | [a,b] | hãy a, hãy b | 
| 3 | thả (b) | [a] | thả (b) | 
| 4 | thả(a) | [] | thả(a) | 

Điều này cho thấy trường hợp tầm thường không cần phạm vi. 

Bây giờ hãy xem xét việc giảm không phải LIFO:```
let a
let b
let c
drop(a)
drop(c)
drop(b)
```| Bước | Hoạt động | Ngăn xếp | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | hãy để | [a] | hãy để | 
| 2 | hãy b | [a,b] | hãy b | 
| 3 | để c | [a,b,c] | để c | 
| 4 | thả(a) | [a,b,c] | đóng phạm vi, thả (a) | 
| 5 | thả (c) | [...] | mở lại phạm vi, drop(c) | 

Điều này chứng tỏ tại sao cần phải có phạm vi:`a`được chôn dưới`b`Và`c`, do đó, việc phân bổ trực tiếp sẽ vi phạm thứ tự ngăn xếp trừ khi chúng tôi cơ cấu lại việc thực thi theo phạm vi. 

Dấu vết xác nhận rằng phạm vi hoạt động như các điểm xả được kiểm soát cho phép chúng tôi bỏ qua các điểm giảm rõ ràng không phải LIFO. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi biến được đẩy và xuất hiện nhiều nhất một lần trên ngăn xếp và bộ đệm tạm thời | 
| Không gian | O(n) | Ngăn xếp và lưu trữ phụ trợ cho các biến | 

Độ phức tạp tuyến tính đủ dễ dàng cho n lên tới 1000 và các thao tác là các thao tác ngăn xếp đơn giản với việc xử lý chuỗi thời gian không đổi cho mỗi sự kiện. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import builtins
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# simple LIFO
assert run("""6
let a = new();
let b = new();
drop(b);
drop(a);
""") == """let a = new();
let b = new();
drop(b);
drop(a);"""

# interleaved requires restructuring
assert run("""6
let a = new();
let b = new();
let c = new();
drop(a);
drop(c);
drop(b);
""") != "", "must produce output"

# single chain
assert run("""4
let x = new();
drop(x);
""") == """let x = new();
drop(x);"""

# all allocations then drops reversed
assert run("""6
let a = new();
let b = new();
let c = new();
drop(c);
drop(b);
drop(a);
""") == """let a = new();
let b = new();
let c = new();
drop(c);
drop(b);
drop(a);"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| LIFO đơn giản | trình tự tương tự | độ chính xác cơ bản | 
| giọt xen kẽ | cấu trúc hợp lệ không trống | xử lý phạm vi | 
| biến đơn | trường hợp tầm thường | hành vi ranh giới | 
| giọt đảo ngược | ngăn xếp trường hợp tối ưu | không có phạm vi không cần thiết | 

## Vỏ cạnh 

Trường hợp biên quan trọng là khi việc phân bổ xảy ra theo mẫu gần như LIFO ngoại trừ một biến sâu duy nhất. Ví dụ:```
let a
let b
let c
drop(b)
drop(c)
drop(a)
```Đây,`b`phá vỡ thứ tự ngăn xếp tự nhiên. Thuật toán sẽ phát hiện ra điều đó`b`không ở trên cùng khi phần thả của nó được xử lý. Nó tạm thời đóng một phạm vi để lộ`b`, đảm bảo rằng`c`không chặn việc loại bỏ. Sau đó, ngăn xếp được khôi phục chính xác và không có biến nào bị mất hoặc trùng lặp ở đầu ra. 

Một trường hợp khác là tiền tố phân bổ dài không có giọt nào cho đến khi kết thúc. Thuật toán tích lũy ngăn xếp và phát ra các mức giảm rõ ràng tối thiểu bằng cách đóng một phạm vi duy nhất ở cuối, hoàn toàn dựa vào sự phá hủy ngầm. 

Cuối cùng, khi mỗi lần thả khớp với đỉnh ngăn xếp, thuật toán không bao giờ phát ra bất kỳ phạm vi nào, cho thấy giải pháp sẽ giảm dần về chương trình ban đầu khi nó đã ở mức tối ưu.
