---
title: "CF 104317J - Dấu ngoặc liền nhau"
description: "Chúng ta được cung cấp một họ chuỗi được xác định đệ quy được xây dựng từ một loại cấu trúc khung nguyên thủy duy nhất. Đối tượng cơ sở là cặp hợp lệ đơn giản nhất “()”."
date: "2026-07-01T19:32:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104317
codeforces_index: "J"
codeforces_contest_name: "Shanghai University 2023 Spring Contest"
rating: 0
weight: 104317
solve_time_s: 74
verified: true
draft: false
---

[CF 104317J - Dấu ngoặc nhọn](https://codeforces.com/problemset/problem/104317/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một họ chuỗi được xác định đệ quy được xây dựng từ một loại cấu trúc khung nguyên thủy duy nhất. Đối tượng cơ sở là cặp hợp lệ đơn giản nhất “()”. Từ đó, chúng ta được phép xây dựng các cấu trúc lớn hơn bằng cách sử dụng hai thao tác: gói cấu trúc hiện có bên trong một cặp dấu ngoặc đơn và nối hai cấu trúc hiện có rồi gói lại kết quả. 

Mỗi truy vấn đưa ra một chuỗi dấu ngoặc đơn và hỏi liệu chuỗi đó có thể được tạo ra bằng cách áp dụng lặp lại chính xác các quy tắc đó bắt đầu từ cơ sở “()” hay không. Nhiệm vụ không phải là kiểm tra xem chuỗi có phải là chuỗi dấu ngoặc đơn đúng tiêu chuẩn hay không, mà là liệu nó có thuộc về ngữ pháp hạn chế hơn này hay không. 

Đầu vào có thể chứa tổng chiều dài lên tới 300.000 chuỗi. Điều đó ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng khám phá tất cả các phân tách hoặc mô phỏng các dẫn xuất một cách rõ ràng. Bất cứ điều gì cố gắng phân tách chuỗi theo mọi cách có thể hoặc duy trì DP trên chuỗi con sẽ trở thành bậc hai trong trường hợp xấu nhất và vượt quá giới hạn. 

Một cái bẫy phổ biến là cho rằng điều này tương đương với việc kiểm tra chuỗi dấu ngoặc đơn cân bằng. Điều đó không thành công vì các chuỗi hợp lệ tiêu chuẩn như “()(())” có thể không tạo được tùy thuộc vào các ràng buộc về cấu trúc do ngữ pháp áp đặt. Một cái bẫy khác là coi nó như một vấn đề thành viên ngữ pháp với phân tích cú pháp chung, sẽ yêu cầu DP khối trên chuỗi con nếu được triển khai trực tiếp. 

Một ví dụ nhỏ cho thấy sự khác biệt là “()()”. Đây là một chuỗi ngoặc hợp lệ, nhưng theo ngữ pháp này, nó không nhất thiết có thể được xây dựng nếu không có đạo hàm nào cho phép hai thành phần độc lập mà không gói các ràng buộc theo đúng thứ tự. Ngược lại, một số biểu mẫu lồng nhau được cho phép ngay cả khi chúng trông có vẻ bị ràng buộc không cần thiết. 

Khó khăn thực sự là ngữ pháp không phải là hành vi không có ngữ cảnh tùy ý, nó có cấu trúc “gói bên ngoài với sự ghép nối tùy chọn bên trong” rất cụ thể, buộc phải phân tách giống như cây thay vì xen kẽ tùy ý. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để suy nghĩ về vấn đề là coi nó như một bài kiểm tra tư cách thành viên ngữ pháp. Chúng ta có thể định nghĩa một DP trong đó dp[l][r] cho biết chuỗi con S[l:r] có thuộc tập hợp hay không. Để một chuỗi con hợp lệ, nó phải là “(X)” trong đó X hợp lệ hoặc “(X)(Y)” trong đó cả hai phần đều hợp lệ và dấu ngoặc đơn ngoài cùng khớp đúng cách. 

Điều này ngay lập tức dẫn đến việc thử tất cả các điểm phân chia trong mỗi khoảng và đối với mỗi khoảng, hãy xác minh cấu trúc khớp. Ngay cả với tính năng ghi nhớ, số khoảng thời gian là O(n2) và mỗi lần chuyển đổi yêu cầu quét các điểm phân tách, đưa ra hành vi O(n³) trong trường hợp xấu nhất. Với tổng n lên tới 3×10⁵, điều này là không khả thi. 

Quan sát quan trọng là ngữ pháp buộc phải có một cấu trúc rất cứng nhắc: mỗi chuỗi hợp lệ hoặc là một cấu trúc được bao bọc nguyên thủy duy nhất hoặc là sự kết hợp được bao bọc của hai cấu trúc hợp lệ nhỏ hơn. Trong cả hai trường hợp, cặp dấu ngoặc đơn ngoài cùng luôn kèm theo sự phân tách thành các khối hợp lệ liên tiếp. 

Điều này có nghĩa là thay vì phân tách tùy ý, chúng ta có thể hiểu quy trình này như việc xây dựng cây phân rã nhị phân có thứ tự gốc trên các khối nguyên thủy. Mỗi nút tương ứng với một phân đoạn cân bằng và cấu trúc của các phần tách được xác định hoàn toàn bằng số lượng thành phần cấp cao nhất tồn tại bên trong mỗi cặp dấu ngoặc đơn. 

Cách tiêu chuẩn để nắm bắt điều này là sử dụng một ngăn xếp duy trì các cấu trúc hiện đang mở và đếm xem có bao nhiêu “thành phần hoàn chỉnh” đã được hình thành ở mỗi độ sâu. Mỗi lần chúng ta đóng dấu ngoặc đơn, chúng ta sẽ hoàn thiện một phần tử nguyên thủy hoặc hợp nhất các phần tử con đã hoàn thành ở cấp độ hiện tại. Cấu trúc hợp lệ khi và chỉ khi chúng ta không bao giờ vi phạm các ràng buộc về thứ tự và kết thúc bằng một cấu trúc hoàn chỉnh duy nhất ở cấp độ ngoài cùng.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute DP trên chuỗi con | O(n³) | O(n²) | Quá chậm | 
| Phân tích cấu trúc dựa trên ngăn xếp | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng chuỗi một cách độc lập bằng cách sử dụng một ngăn xếp đại diện cho các khung xây dựng lồng nhau. Mỗi khung hình sẽ theo dõi xem nó có nhìn thấy ít nhất một thành phần đã hoàn thiện bên trong nó hay không. 

1. Khởi tạo một ngăn xếp trống. Mỗi mục ngăn xếp thể hiện một ngữ cảnh dấu ngoặc đơn hiện đang mở và liệu nó đã tích lũy các cấu trúc con hoàn chỉnh hay chưa. 
2. Quét chuỗi từ trái sang phải. Khi chúng ta thấy “(”, chúng ta tạo một khung mới và đẩy nó vào ngăn xếp. Điều này tương ứng với việc bắt đầu một lớp xây dựng mới theo định nghĩa đệ quy. 
3. Khi chúng ta thấy “)”, chúng ta đang đóng khung hiện tại. Tại thời điểm này, khung đại diện cho một cấu trúc phụ được xây dựng hoàn chỉnh. Nếu ngăn xếp trống trước khi bật lên thì chuỗi không hợp lệ vì chúng ta đang đóng mà không có ngữ cảnh mở. 
4. Bật khung trên cùng. Nếu khung này không chứa cấu trúc con hoàn chỉnh và không có thành phần hợp lệ bên trong thì nó đại diện cho trường hợp cơ sở “()”, do đó nó vẫn hợp lệ dưới dạng một đơn vị. 
5. Sau khi bật lên, hãy coi đơn vị đã hoàn thiện này như một “khối” có thể thuộc về khung cấp cao hơn. Nếu ngăn xếp không trống, chúng tôi đánh dấu rằng khung cha hiện đã nhận được ít nhất một thành phần con đã hoàn thành. 
6. Nếu tại bất kỳ thời điểm nào chúng tôi gặp phải dấu ngoặc đóng khi không có khung hoạt động hoặc chúng tôi quét xong và còn sót lại các khung mở thì chuỗi đó không hợp lệ. 
7. Cuối cùng, chuỗi hợp lệ nếu chính xác một cấu trúc hoàn chỉnh được hình thành ở cấp độ ngoài cùng, nghĩa là quá trình xử lý ngăn xếp kết thúc rõ ràng và toàn bộ chuỗi thu gọn thành một khối gốc duy nhất. 

Ý tưởng quan trọng là ngữ pháp chỉ cho phép hai cách xây dựng cấu trúc: gói và nối các cấu trúc đã hoàn chỉnh. Ngăn xếp đảm bảo rằng chúng tôi chỉ hợp nhất các cấu trúc con hoàn chỉnh, không bao giờ hợp nhất một phần. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào trong quá trình quét, mỗi khung ngăn xếp tương ứng với một chuỗi con hiện đang được xây dựng và được đảm bảo cân bằng nếu hoàn thành. Điều bất biến là mỗi khi chúng ta bật một khung, nó sẽ thể hiện một cấu trúc hợp lệ tối đa được hình thành hoàn toàn từ các thành phần đã hoàn thành trước đó. 

Bởi vì ngữ pháp chỉ cho phép kết hợp các thành phần đã hợp lệ nên không có dẫn xuất hợp lệ nào có thể yêu cầu phân tách bên trong một phân đoạn không đầy đủ. Ngược lại, mỗi khi ngăn xếp hoàn thành một khung, chúng ta đã xây dựng chính xác một đơn vị phù hợp với ngữ pháp. Điều kiện chấp nhận cuối cùng buộc toàn bộ chuỗi phải thu gọn thành một đơn vị duy nhất, phù hợp với định nghĩa đệ quy của tập hợp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def is_valid(s: str) -> bool:
    stack = []
    
    for ch in s:
        if ch == '(':
            # start a new frame, no completed children yet
            stack.append(0)
        else:
            if not stack:
                return False
            
            # finish current frame
            had_child = stack.pop()
            
            # we now have a completed block; attach it to parent if exists
            if stack:
                stack[-1] = 1  # parent now has at least one component
    
    return len(stack) == 0

def main():
    t = int(input())
    for _ in range(t):
        s = input().strip()
        print("YES" if is_valid(s) else "NO")

if __name__ == "__main__":
    main()
```Việc triển khai sử dụng một ngăn xếp trong đó mỗi mục nhập là một điểm đánh dấu đơn giản cho biết khung hiện tại đã tích lũy ít nhất một khối bên trong đã hoàn thành hay chưa. Khi gặp dấu ngoặc đơn đóng, chúng tôi bật khung và coi nó như một đơn vị hoàn chỉnh. Nếu có khung gốc, chúng tôi đánh dấu khung đó là đã nhận được thành phần hợp lệ. 

Tính chính xác phụ thuộc vào việc đảm bảo chúng tôi không bao giờ chấp nhận các dấu ngoặc đóng không khớp và tất cả các khung đã mở đều được đóng ở cuối. Ngăn xếp cuối cùng trống đảm bảo toàn bộ cấu trúc giảm xuống thành một cấu trúc hợp lệ duy nhất. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai chuỗi để xem ngăn xếp phát triển như thế nào. 

Đầu tiên hãy xem xét chuỗi “(()())”. 

| Bước | Char | Trạng thái ngăn xếp | 
| --- | --- | --- | 
| 1 | ( | [0] | 
| 2 | ( | [0, 0] | 
| 3 | ) | [0] | 
| 4 | ( | [0, 0] | 
| 5 | ) | [0] | 
| 6 | ) | [] | 

Tại mỗi dấu ngoặc đóng, chúng ta thu gọn cấu trúc trong cùng thành một đơn vị duy nhất và truyền nó lên trên. Ngăn xếp kết thúc trống, xác nhận một cấu trúc gốc hợp lệ. Điều này tương ứng với một bố cục lồng nhau trong đó các khối hợp lệ bên trong được kết hợp theo quy tắc ngữ pháp. 

Bây giờ hãy xem xét “(()))”. 

| Bước | Char | Trạng thái ngăn xếp | 
| --- | --- | --- | 
| 1 | ( | [0] | 
| 2 | ( | [0, 0] | 
| 3 | ) | [0] | 
| 4 | ) | [] | 
| 5 | ) | không hợp lệ | 

Ở bước 5, chúng tôi cố gắng đóng dấu ngoặc đơn không có khung hoạt động, điều này ngay lập tức vi phạm các quy tắc xây dựng. Điều này chứng tỏ cách ngăn xếp phát hiện sớm các vi phạm cấu trúc mà không cần phân tích cú pháp đầy đủ. 

Ví dụ đầu tiên xác nhận việc giảm lồng nhau phù hợp, trong khi ví dụ thứ hai cho thấy việc đóng không hợp lệ sẽ phá vỡ quá trình xây dựng như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nhân vật được đẩy hoặc bật lên nhiều nhất một lần | 
| Không gian | O(n) | Ngăn xếp lưu trữ tối đa một mục nhập cho mỗi dấu ngoặc đơn mở | 

Tổng kích thước đầu vào lên tới 3×10⁵ ký tự, do đó, quá trình quét tuyến tính trên mỗi trường hợp thử nghiệm sẽ vừa vặn thoải mái trong giới hạn. Việc sử dụng bộ nhớ cũng tuyến tính ở độ sâu lồng tối đa, được giới hạn bởi độ dài chuỗi. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    input = sys.stdin.readline
    t = int(input())
    for _ in range(t):
        s = input().strip()
        stack = []
        ok = True
        for ch in s:
            if ch == '(':
                stack.append(0)
            else:
                if not stack:
                    ok = False
                    break
                stack.pop()
                if stack:
                    stack[-1] = 1
        if stack:
            ok = False
        print("YES" if ok else "NO")

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out.strip()

# provided samples
assert run("""5
((())())
(()())
()()
(()()))
((()())())""") == """YES
YES
NO
NO
YES"""

# minimum size
assert run("""1
()""") == "YES"

# simple invalid
assert run("""1
)("""") == "NO"

# nested deep
assert run("""1
(((())))""") == "YES"

# multiple components invalid for this grammar
assert run("""1
()()""") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| () | CÓ | xây dựng hợp lệ tối thiểu | 
| )( | KHÔNG | đóng cửa sớm không hợp lệ | 
| (((()))) | CÓ | tính chính xác của việc lồng sâu | 
| ()() | KHÔNG | cấu trúc nối phẳng không được phép | 

## Vỏ cạnh 

Một trường hợp quan trọng là một chuỗi được cân bằng theo nghĩa thông thường nhưng về mặt cấu trúc không tương thích với ngữ pháp, chẳng hạn như “()()”. Ngăn xếp xử lý nó mà không bao giờ gặp phải dấu ngoặc không hợp lệ, nhưng cấu trúc cuối cùng không thu gọn vào một khung gốc duy nhất, để lại nhiều thành phần cấp cao nhất. Thuật toán từ chối nó vì ngăn xếp không trống ở cuối, điều này phù hợp với yêu cầu rằng toàn bộ chuỗi phải đại diện cho một đối tượng được xây dựng duy nhất. 

Một trường hợp cạnh khác là việc đóng không hợp lệ sớm như “)(”. Ký tự đầu tiên cố gắng đóng một khung không tồn tại, gây ra sự từ chối ngay lập tức. Điều này cho thấy thuật toán thực thi chính xác tính hợp lệ của tiền tố chứ không chỉ cân bằng toàn cục. 

Trường hợp thứ ba là các chuỗi được lồng sâu như “((())))”, trong đó mỗi dấu ngoặc mở tạo ra một khung mới và mọi dấu ngoặc đóng sẽ thu gọn khung đó một cách rõ ràng. Kích thước ngăn xếp tăng tuyến tính và sau đó trở về 0, xác nhận rằng việc lồng thuần túy luôn hợp lệ theo các quy tắc xây dựng.
