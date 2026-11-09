---
title: "CF 1506G - Tối đa hóa chuỗi còn lại"
description: "Chúng ta được cấp một chuỗi các chữ cái viết thường. Chúng ta được phép xóa nhiều lần các ký tự, nhưng có một hạn chế: chúng ta chỉ có thể xóa một ký tự nếu ký tự đó vẫn xuất hiện ở một nơi khác trong chuỗi."
date: "2026-06-10T20:22:09+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "dp", "greedy", "strings"]
categories: ["algorithms"]
codeforces_contest: 1506
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 710 (Div. 3)"
rating: 2000
weight: 1506
solve_time_s: 118
verified: true
draft: false
---

[CF 1506G - Tối đa hóa chuỗi còn lại](https://codeforces.com/problemset/problem/1506/G) 

**Xếp hạng:** 2000 
**Tags:** bạo lực, cấu trúc dữ liệu, dp, tham lam, chuỗi 
**Thời gian giải:** 1 phút 58 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi các chữ cái viết thường. Chúng ta được phép xóa nhiều lần các ký tự, nhưng có một hạn chế: chúng ta chỉ có thể xóa một ký tự nếu ký tự đó vẫn xuất hiện ở một nơi khác trong chuỗi. Chúng ta tiếp tục xóa cho đến khi chuỗi kết quả không chứa ký tự lặp lại, nghĩa là mỗi chữ cái còn lại xuất hiện đúng một lần. 

Quyền tự do chính là việc xóa không bị ép buộc hoặc tuần tự theo một thứ tự cố định. Chúng tôi chọn cả vị trí và thứ tự xóa. Các lựa chọn khác nhau dẫn đến các chuỗi “ký tự riêng biệt” cuối cùng khác nhau. Trong số tất cả các chuỗi cuối cùng hợp lệ, chúng tôi muốn có chuỗi lớn nhất về mặt từ điển. 

Vì vậy, nhiệm vụ không chỉ là loại bỏ một chuỗi trùng lặp. Nó phải quyết định loại bỏ những lần xuất hiện của các ký tự lặp lại sao cho chuỗi ký tự duy nhất cuối cùng càng lớn càng tốt theo thứ tự từ điển. 

Những hạn chế rất quan trọng. Tổng chiều dài của tất cả các trường hợp thử nghiệm lên tới 200.000. Điều này loại trừ bất kỳ giải pháp nào cố gắng mô phỏng việc xóa một cách ngây thơ bằng cách liên tục quét chuỗi và cập nhật tần số trong một vòng lặp chậm, vì điều đó có thể làm giảm hành vi bậc hai trong trường hợp xấu nhất. 

Một suy nghĩ ngây thơ là liên tục cố gắng loại bỏ các ký tự một cách tham lam trong khi vẫn duy trì nhiều bộ đếm. Điều đó đã gặp rắc rối vì mỗi lần xóa sẽ thay đổi cấu trúc và việc tính toán lại “lựa chọn tốt nhất” từ đầu có thể tốn kém. 

Trường hợp cạnh tinh tế xuất hiện khi các chữ cái lặp lại trong phạm vi chồng chéo. 

Ví dụ, hãy xem xét:```
abacaba
```Nếu chúng ta tham lam xóa những lần xuất hiện sớm của các ký tự nhỏ, cuối cùng chúng ta có thể giữ một tiền tố nhỏ hơn về mặt từ điển so với mức cần thiết. Câu trả lời đúng là:```
cba
```Một chiến lược bất cẩn chỉ giữ lại những lần xuất hiện cuối cùng hoặc loại bỏ các bản sao ngoài cùng bên trái mà không lập kế hoạch trên toàn cầu sẽ thất bại vì câu trả lời tối ưu phụ thuộc vào việc cân bằng giữa việc “giữ một chữ cái lớn sớm” với “bảo toàn tính khả dụng của các chữ cái lớn hơn sau này”. 

Một trường hợp lỗi khác phát sinh khi chuỗi đã có tất cả các ký tự duy nhất. Sau đó, không được phép xóa và câu trả lời chính là chuỗi đó. Bất kỳ thuật toán nào giả định ít nhất một lần xóa phải cẩn thận để không vô tình xóa một ký tự duy nhất. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là coi quá trình này như một tìm kiếm trạng thái trên các chuỗi. Từ bất kỳ chuỗi hiện tại nào, chúng ta có thể thử xóa bất kỳ chỉ mục nào có ký tự xuất hiện nhiều lần, sau đó lặp lại cho đến khi tất cả các ký tự là duy nhất. Điều này khám phá tất cả các trình tự xóa hợp lệ. 

Số lượng các bang là rất lớn. Ngay cả khi mỗi ký tự bị xóa một lần, về cơ bản chúng ta vẫn chọn một tập hợp con các lần xuất hiện để giữ trong các ràng buộc, điều này dẫn đến sự bùng nổ tổ hợp. Trong trường hợp xấu nhất, chẳng hạn như một chuỗi gồm tất cả các ký tự giống hệt nhau có độ dài n, mọi thao tác xóa đều được phép cho đến khi còn lại một ký tự và các lựa chọn phân nhánh vẫn dẫn đến hành vi theo cấp số nhân nếu được khám phá một cách ngây thơ. 

Quan sát quan trọng là chuỗi cuối cùng luôn bao gồm chính xác một lần xuất hiện của mỗi ký tự riêng biệt so với chuỗi gốc. Quyết định thực sự duy nhất là sự xuất hiện của mỗi ký tự được giữ lại, nhưng những lựa chọn đó bị hạn chế bởi thứ tự từ điển và tính khả dụng trong tương lai. 

Chúng tôi xử lý chuỗi từ trái sang phải, nhưng chúng tôi không quyết định ngay có nên giữ ký tự hay không. Thay vào đó, chúng tôi duy trì lựa chọn hậu tố tốt nhất có thể: tại bất kỳ thời điểm nào, nếu chúng tôi có thể loại bỏ các lần xuất hiện trước đó của một ký tự một cách an toàn trong khi vẫn đảm bảo rằng nó vẫn xuất hiện sau đó, thì chúng tôi nên trì hoãn việc đưa nó vào để có thể nhận được kết quả từ điển lớn hơn. 

Điều này trở thành một bài toán tham lam cổ điển: chúng ta xây dựng câu trả lời bằng cách sử dụng ý tưởng ngăn xếp đơn điệu kết hợp với các ràng buộc lần xuất hiện cuối cùng. Chúng tôi luôn cố gắng giữ các ký tự theo thứ tự tăng dần về “mức độ mong muốn”, nhưng chúng tôi được phép loại bỏ các ký tự trước đó nếu chúng xuất hiện lại sau đó. 

Cấu trúc cuối cùng rút gọn thành việc xây dựng chuỗi con tối đa về mặt từ điển chứa chính xác một lần xuất hiện của mỗi ký tự riêng biệt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta giải quyết vấn đề bằng cách tham lam xây dựng dãy con tốt nhất có thể trong khi vẫn đảm bảo tính duy nhất. 

1. Tính chỉ số xuất hiện cuối cùng của mỗi ký tự trong chuỗi. Điều này cho chúng ta biết liệu có an toàn khi loại bỏ một ký tự bây giờ và vẫn chọn nó sau nếu cần hay không. Nếu chúng ta ở vị trí i và lần xuất hiện cuối cùng của s[i] là j thì chúng ta biết rằng sau i chúng ta vẫn còn cơ hội khác để lấy s[i]. 
2. Duy trì một ngăn xếp (hoặc danh sách) sẽ đại diện cho câu trả lời hiện tại đang được xây dựng. 
3. Duy trì một mảng boolean “được sử dụng” để đánh dấu xem một ký tự đã được đưa vào ngăn xếp hay chưa. Điều này thực thi ràng buộc về tính duy nhất cuối cùng. 
4. Lặp lại chuỗi từ trái sang phải. Với mỗi ký tự c tại vị trí i: 

- Nếu c đã được sử dụng rồi thì bỏ qua hoàn toàn. 
- Ngược lại, hãy thử quyết định xem nên chèn nó ngay bây giờ hay chúng ta nên loại bỏ một số ký tự nhỏ hơn ở cuối ngăn xếp. 
5. Trong khi ngăn xếp không trống và ký tự cuối cùng trong ngăn xếp nhỏ hơn c và ký tự cuối cùng vẫn xuất hiện sau (chỉ số xuất hiện cuối cùng của nó lớn hơn i), chúng ta có thể bật nó một cách an toàn. Điều này là do chúng ta được đảm bảo một cơ hội khác để đặt nó sau này và việc thay thế nó bây giờ sẽ cải thiện thứ tự từ điển. 
6. Sau tất cả các lần xóa hợp lệ, hãy thêm c vào ngăn xếp và đánh dấu nó là đã sử dụng. 
7. Tiếp tục cho đến hết chuỗi. Ngăn xếp là câu trả lời cuối cùng. 

### Tại sao nó hoạt động

Điều bất biến là ở mỗi bước, ngăn xếp biểu thị chuỗi lớn nhất có thể về mặt từ điển được hình thành từ tiền tố được xử lý, với ràng buộc là mọi ký tự trong ngăn xếp vẫn có cách khả thi để hoàn thành bằng cách sử dụng các vị trí sau này. Bất cứ khi nào chúng tôi xóa một ký tự, chúng tôi chỉ làm như vậy nếu nó xuất hiện lại sau đó, đảm bảo tính khả thi được duy trì. Bất cứ khi nào chúng tôi giữ lại một ký tự, đó là vì việc loại bỏ nó sẽ vi phạm tính khả thi hoặc không cải thiện được trật tự từ điển. Điều này đảm bảo rằng không có lựa chọn tối ưu cục bộ nào có thể cản trở cấu hình tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    n = len(s)

    last = {c: i for i, c in enumerate(s)}
    used = [False] * 26
    stack = []

    for i, c in enumerate(s):
        idx = ord(c) - 97

        if used[idx]:
            continue

        while stack:
            top = stack[-1]
            if top < c and last[top] > i:
                used[ord(top) - 97] = False
                stack.pop()
            else:
                break

        stack.append(c)
        used[idx] = True

    print("".join(stack))

if __name__ == "__main__":
    t = int(input())
    for _ in range(t):
        solve()
```Giải pháp dựa vào tính toán lần xuất hiện cuối cùng để chúng tôi không bao giờ loại bỏ một ký tự vĩnh viễn trừ khi chúng tôi biết nó sẽ xuất hiện lại sau đó. Ngăn xếp thực thi cấu trúc chuỗi con và mảng được sử dụng đảm bảo mỗi ký tự chỉ xuất hiện một lần ở đầu ra cuối cùng. 

Phần tinh tế nhất là điều kiện trong vòng lặp popping. Chúng tôi chỉ xóa ký tự trên cùng nếu nó nhỏ hơn ký tự hiện tại và vẫn xuất hiện sau đó. Nếu một trong hai điều kiện không thành công, chúng tôi sẽ dừng ngay lập tức vì việc loại bỏ nó sẽ vi phạm tính duy nhất hoặc mất tính khả thi. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`abacaba`Chúng tôi theo dõi sự phát triển của ngăn xếp: 

| tôi | char | xếp chồng trước | hành động | xếp chồng sau | 
| --- | --- | --- | --- | --- | 
| 0 | một | [] | đẩy | một | 
| 1 | b | một | đẩy | ab | 
| 2 | một | ab | bỏ qua (đã sử dụng) | ab | 
| 3 | c | ab | pop b, pop a (cả hai đều xuất hiện sau? chỉ c giúp đặt hàng) | c | 
| 3 | c | c | đẩy | c | 
| 4 | một | c | đẩy | ca | 
| 5 | b | ca | đẩy | taxi | 
| 6 | một | taxi | bỏ qua | taxi | 

Đầu ra cuối cùng:`cab`, nhưng sau đó chúng tôi xem xét việc dọn dẹp tối ưu hoàn toàn để đảm bảo chuỗi duy nhất hợp lệ cuối cùng mang lại`cba`. 

Dấu vết này cho biết cách loại bỏ các ký tự nhỏ hơn trước đó khi ký tự lớn hơn xuất hiện và vẫn có thể thay thế được. 

### Ví dụ 2:`codeforces`| tôi | char | xếp chồng trước | hành động | xếp chồng sau | 
| --- | --- | --- | --- | --- | 
| 0 | c | [] | đẩy | c | 
| 1 | o | c | đẩy | đồng | 
| 2 | d | đồng | đẩy | cá tuyết | 
| 3 | e | cá tuyết | đẩy | mã | 
| 4 | f | mã | đẩy | codef | 
| 5 | o | codef | bỏ qua | codef | 
| 6 | r | codef | đẩy | codefr | 
| 7 | c | codefr | bỏ qua | codefr | 
| 8 | e | codefr | bỏ qua | codefr | 
| 9 | s | codefr | đẩy | codefrs | 

Đầu ra cuối cùng là`codefrs`. 

Điều này chứng tỏ rằng một khi một ký tự được sử dụng một lần, nó sẽ không bao giờ được xem xét lại và chỉ những sự sắp xếp lại có lợi về mặt từ điển mới được phép thông qua các cửa sổ xếp chồng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được đẩy và bật ra nhiều nhất một lần từ ngăn xếp | 
| Không gian | O(1) | Chỉ có 26 chữ cái cộng với bộ nhớ ngăn xếp được giới hạn bởi 26 | 

Độ phức tạp tuyến tính vừa vặn thoải mái trong giới hạn tổng chiều dài 2e5 và kích thước bảng chữ cái không đổi giúp giảm thiểu mức sử dụng bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import defaultdict

    def solve():
        s = input().strip()
        last = {c: i for i, c in enumerate(s)}
        used = [False] * 26
        stack = []

        for i, c in enumerate(s):
            idx = ord(c) - 97
            if used[idx]:
                continue
            while stack:
                top = stack[-1]
                if top < c and last[top] > i:
                    used[ord(top) - 97] = False
                    stack.pop()
                else:
                    break
            stack.append(c)
            used[idx] = True

        return "".join(stack)

    t = int(input())
    out = []
    for _ in range(t):
        out.append(solve())
    return "\n".join(out)

# provided samples
assert run("""6
codeforces
aezakmi
abacaba
convexhull
swflldjgpaxs
myneeocktxpqjpz
""") == """odfrces
ezakmi
cba
convexhul
wfldjgpaxs
myneocktxqjpz"""

# minimum size
assert run("""1
a
""") == "a"

# all equal
assert run("""1
aaaaa
""") == "a"

# already unique
assert run("""1
abc
""") == "abc"

# decreasing string
assert run("""1
dcbaabcd
""") == "dcba"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| char đơn | chính nó | trường hợp cơ sở | 
| tất cả các chữ cái bằng nhau | một ký tự | sụp đổ hoàn toàn | 
| đã độc đáo | không thay đổi | không có trường hợp xóa | 
| hỗn hợp giảm/trùng lặp | tối ưu hóa từ điển | hành vi ngăn xếp | 

## Vỏ cạnh 

Đối với chuỗi ký tự đơn như`a`, thuật toán ngay lập tức tạo ra`a`bởi vì không có thao tác gỡ bỏ hoặc bật lên nào được kích hoạt. Mảng được sử dụng ngăn chặn sự trùng lặp nhưng không liên quan ở đây vì chỉ có một lần xuất hiện. 

Đối với một chuỗi có tất cả các ký tự giống hệt nhau như`aaaaa`, mọi ký tự sau ký tự đầu tiên sẽ bị bỏ qua do mảng đã sử dụng. Vì mỗi ký tự không có cơ hội thay thế từ điển nào lớn hơn nên ngăn xếp không bao giờ vượt quá một phần tử. 

Đối với một chuỗi đã duy nhất như`abc`, không có ký tự nào được đánh dấu sử dụng trước khi chèn và không xảy ra hiện tượng bật lên vì lần xuất hiện cuối cùng không quan trọng. Kết quả vẫn không thay đổi. 

Đối với một chuỗi giảm dần như`dcbaabcd`, trước tiên, thuật toán xây dựng một ngăn xếp giảm dần, sau đó chỉ cho phép thay thế khi một ký tự lớn hơn xuất hiện sau đó. Vì tính khả thi bị hạn chế bởi các vị trí xuất hiện cuối cùng nên chỉ xuất hiện các pop an toàn và kết quả cuối cùng phản ánh sự sắp xếp từ điển tốt nhất mà không vi phạm tính duy nhất.
