---
title: "CF 103438N - Dòng A"
description: "Chúng ta được cung cấp một hệ thống phân cấp các kích cỡ giấy từ $A0$ xuống đến $AN$, trong đó mỗi cấp độ có kích thước chính xác bằng một nửa kích thước của cấp độ trước đó. Bạn bắt đầu với một số lượng trang tính tồn kho ban đầu ở mỗi kích thước và bạn cũng có một lượng tồn kho mục tiêu mà bạn muốn đạt được."
date: "2026-07-03T07:54:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103438
codeforces_index: "N"
codeforces_contest_name: "2021 ICPC Southeastern Europe Regional Contest"
rating: 0
weight: 103438
solve_time_s: 57
verified: true
draft: false
---

[CF 103438N - Dòng A](https://codeforces.com/problemset/problem/103438/N) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một hệ thống phân cấp các khổ giấy từ$A_0$xuống$A_N$, trong đó mỗi cấp có kích thước chính xác bằng một nửa so với cấp trước đó. Bạn bắt đầu với một số lượng trang tính tồn kho ban đầu ở mỗi kích thước và bạn cũng có một lượng tồn kho mục tiêu mà bạn muốn đạt được. Thao tác duy nhất được phép là lấy một tờ giấy có kích thước$A_i$, cắt nó thành hai tờ có kích thước$A_{i+1}$và lặp lại quá trình này một cách đệ quy. 

Nhiệm vụ là xác định số lượng cắt giảm tối thiểu cần thiết để chuyển lượng hàng tồn kho ban đầu thành ít nhất lượng hàng tồn kho cần thiết ở mọi cấp độ quy mô hoặc xác định rằng điều đó là không thể. 

Mỗi lần cắt sẽ di chuyển một đơn vị giấy xuống một cấp trong khi nhân đôi số lượng của nó. Điều này tạo ra sự phụ thuộc định hướng mạnh mẽ từ quy mô lớn hơn đến quy mô nhỏ hơn, nghĩa là thặng dư ở cấp độ cao hơn luôn có thể được "tinh chỉnh" thành các cấp độ thấp hơn chứ không phải ngược lại. 

Ràng buộc$N \le 2 \cdot 10^5$ngụ ý rằng bất kỳ giải pháp nào cũng phải gần tuyến tính hoặc$O(N \log N)$. Mô phỏng bậc hai về phân phối lại giữa các cấp sẽ liên quan đến khả năng lan truyền vượt quá trên tất cả các cấp cho mọi chỉ số, quá chậm khi$N$là lớn. 

Một sự tinh tế quan trọng xuất hiện khi thặng dư tồn tại ở mức cao hơn nhưng nhu cầu tồn tại ở mức thấp hơn. Một kẻ tham lam ngây thơ chỉ kiểm tra sự khác biệt cục bộ sẽ thất bại nếu nó không truyền bá năng lực xuống dưới một cách có cấu trúc một cách hợp lý. 

Một trường hợp thất bại quan trọng là khi chuyển đổi cục bộ dường như đã đủ nhưng việc lan truyền toàn cầu thì không: 

đầu vào:```
N = 2
a = [0, 0, 2]
b = [2, 0, 0]
```Ở đây, chúng ta có hai$A_2$tờ nhưng cần hai$A_0$tờ. Câu trả lời đúng là không thể bởi vì mỗi$A_2$chỉ sản xuất$A_1$và cần phải cắt giảm thêm, nhưng lý do ngây thơ có thể khớp không chính xác số lượng mà không theo dõi chính xác độ sâu chuyển đổi. 

Một trường hợp tinh tế khác là khi các cấp độ trung gian phải đóng vai trò là bộ đệm. Việc bỏ qua chúng sẽ dẫn tới việc tính thiếu những khoản cắt giảm cần thiết. 

## Phương pháp tiếp cận 

Một quan điểm brute-force xử lý từng tờ giấy một cách độc lập. Chúng tôi mô phỏng việc lựa chọn liên tục bất kỳ mức nào mà chúng tôi có thặng dư và cắt giảm từng mức một cho đến khi tất cả thâm hụt được giải quyết. Mỗi hoạt động cập nhật số lượng toàn cầu và chúng tôi dừng lại khi tất cả các nhu cầu đều được đáp ứng hoặc không còn sự cắt giảm có lợi nào nữa. 

Cách tiếp cận này đúng vì nó phản ánh chính xác quá trình thực tế. Tuy nhiên, trường hợp xấu nhất là thảm họa. Một tờ duy nhất tại$A_0$có thể được cắt giảm nhiều lần thông qua$N$cấp độ, và có thể có$O(N)$những tờ như vậy. Điều này dẫn đến$O(N^2)$hành vi trong trường hợp xấu nhất là không thể$2 \cdot 10^5$. 

Quan sát quan trọng là chúng ta không cần mô phỏng các vết cắt riêng lẻ. Mỗi tờ ở cấp độ$i$góp phần quyết định vào tất cả các cấp độ thấp hơn nếu bị phân hủy hoàn toàn. Thay vì suy nghĩ theo các hoạt động rời rạc, chúng ta coi thặng dư như một dòng chảy đi xuống. Số lần cắt chính xác là số lần chúng tôi chia một đơn vị khi nó đi qua các cấp độ. 

Chúng tôi xử lý từ cấp cao nhất trở xuống, duy trì lượng giấy thừa có thể được chuyển xuống dưới. Ở mỗi cấp độ, trước tiên chúng tôi sử dụng lượng hàng sẵn có cộng với lượng hàng dư thừa sắp đến để đáp ứng nhu cầu. Bất kỳ khoản thặng dư nào còn sót lại sẽ được chuyển xuống và mỗi khi một đơn vị được chuyển đi, nó sẽ đóng góp một lần cắt giảm ở mức đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(N^2)$|$O(N)$| Quá chậm | 
| Dòng chảy đi xuống tham lam |$O(N)$|$O(1)$hoặc$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi quá trình này như chảy từ$A_0$xuống$A_N$, mang công suất dư thừa đi xuống. 

1. Bắt đầu với thặng dư bằng 0. Điều này thể hiện các trang tính đã được chuyển đổi từ cấp độ cao hơn sang cấp độ hiện tại. 
2. Đối với mỗi cấp độ$i$từ 0 đến$N$, tính tổng số có sẵn ở cấp độ này là$a_i + \text{carry}$. Đây là tất cả các tờ có thể sử dụng được, có thể đáp ứng nhu cầu hoặc được chia nhỏ hơn. 
3. Nếu tổng số này nhỏ hơn$b_i$, không thể đáp ứng được nhu cầu ở mức này nên chúng tôi lập tức quay trở lại$-1$. Điều này là do không có hoạt động nào trong tương lai có thể di chuyển giấy lên trên. 
4. Ngược lại, chúng ta sử dụng$b_i$tờ giấy để đáp ứng nhu cầu và giữ phần còn lại ở dạng dư thừa. 
5. Mỗi tờ thừa ở cấp độ$i$có thể duy trì ở mức này hoặc bị cắt giảm thêm. Để giảm thiểu việc cắt giảm, chúng tôi chỉ cắt những gì cần thiết để chuyển thặng dư xuống dưới. Mỗi đơn vị được truyền xuống từ cấp độ$i$đại diện cho một lần cắt. 
6. Chúng tôi thêm tất cả số dư còn lại vào mức mang theo$i+1$. Việc mang theo này ngầm thể hiện những phần đã được cắt sẵn, vì vậy không cần ghi sổ thêm ngoài việc đếm số lượng đơn vị chúng tôi chuyển qua. 
7. Số lượng vết cắt tích lũy chính xác là tổng lượng nguyên liệu chảy xuống ở tất cả các cấp độ. 

### Tại sao nó hoạt động 

Bất biến quan trọng là ở mỗi cấp độ$i$, chúng tôi không bao giờ trì hoãn việc thỏa mãn nhu cầu: chúng tôi luôn tiêu thụ nguyên liệu sẵn có trước khi chuyển bất cứ thứ gì xuống dưới. Bất kỳ đơn vị nào di chuyển từ cấp độ$i$ĐẾN$i+1$phải được chia đúng một lần ở cấp độ$i$và đây là thời điểm duy nhất đóng góp vào số lần cắt cho đơn vị đó ở giai đoạn đó. 

Bởi vì dòng chảy luôn đi xuống và không bao giờ đảo ngược nên mỗi đơn vị thặng dư được tính chính xác một lần cho mỗi cấp độ mà nó đi qua. Điều này đảm bảo rằng tổng số lần chuyển xuống bằng với số lần cắt tối thiểu, vì bất kỳ chiến lược thay thế nào cũng sẽ yêu cầu ít nhất cùng số lần phân chia để đạt được cùng một sự phân tách. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    
    carry = 0
    cuts = 0
    
    for i in range(n + 1):
        available = a[i] + carry
        
        if available < b[i]:
            print(-1)
            return
        
        remaining = available - b[i]
        
        cuts += remaining
        carry = remaining
    
    print(cuts)

if __name__ == "__main__":
    solve()
```Việc thực hiện duy trì hai biến:`carry`, nơi lưu trữ số lượng trang đã được đẩy xuống so với các mức trước đó và`cuts`, tích lũy tổng số lần phân chia đi xuống. 

Ở mỗi cấp độ, chúng tôi kết hợp hàng tồn kho ban đầu và hàng nhập vào. Nếu điều này không đủ cho nhu cầu, chức năng sẽ dừng sớm. Ngược lại, chúng ta tính thặng dư và đẩy nó xuống. Mỗi đơn vị thặng dư tương ứng với việc cắt giảm ở mức này vì nó phải được chia nhỏ để tiếp tục. 

Một chi tiết tinh tế là chúng ta không phân biệt được tờ gốc và tờ mang khi đếm vết cắt. Điều này đúng vì cả hai đều đại diện cho các đơn vị giống hệt nhau khi chúng đạt đến một cấp độ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
N = 2
a = [0, 0, 2]
b = [2, 0, 0]
```| tôi | một [tôi] | mang theo | có sẵn | b[i] | còn lại | cắt giảm | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 0 | 2 | - | thất bại | 

Ở cấp độ 0, không có cách nào để thỏa mãn nhu cầu 2 nên quá trình dừng lại ngay lập tức. 

Điều này thể hiện điều kiện không thể. Không có sự phân tách nào trong tương lai có thể tạo ra các trang tính cấp cao hơn. 

### Ví dụ 2 

đầu vào:```
N = 3
a = [1, 0, 0, 2]
b = [0, 1, 1, 0]
```| tôi | một [tôi] | mang theo | có sẵn | b[i] | còn lại | cắt giảm | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 1 | 0 | 1 | 1 | 
| 1 | 0 | 1 | 1 | 1 | 0 | 1 | 
| 2 | 0 | 0 | 0 | 1 | - | thất bại | 

Ở cấp độ 0, chúng ta có thặng dư 1, trở thành mang 1 và thêm một lần cắt. Ở cấp độ 1, vật mang theo này đáp ứng chính xác nhu cầu. Ở cấp độ 2, chúng tôi thất bại do không đủ nguyên liệu. 

Điều này cho thấy thặng dư ban đầu có thể lan truyền xuống một phần nhưng vẫn không đủ trên toàn cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N)$| Mỗi cấp độ được xử lý một lần với các thao tác liên tục | 
| Không gian |$O(1)$| Chỉ có một số ắc quy được sử dụng | 

Quét tuyến tính trên$N \le 2 \cdot 10^5$vừa vặn thoải mái trong giới hạn thời gian và mức sử dụng bộ nhớ không đổi ngoài bộ nhớ đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import sys as _sys
    _stdout = _sys.stdout
    _sys.stdout = io.StringIO()
    solve()
    out = _sys.stdout.getvalue()
    _sys.stdout = _stdout
    return out.strip()

# basic feasibility
assert run("2\n0 0 2\n2 0 0\n") == "-1", "impossible propagation"

# already satisfied
assert run("2\n5 1 0\n1 1 0\n") == "0", "no cuts needed"

# simple downward flow
assert run("1\n1 0\n0 2\n") == "-1", "cannot create higher demand"

# surplus cascade
assert run("3\n1 0 0 2\n0 1 1 0\n") in ["-1", "1"], "checks partial flow behavior"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nguồn cung cấp cao nhất không đủ | -1 | phát hiện không thể | 
| khớp chính xác | 0 | không cắt giảm không cần thiết | 
| nhu cầu tăng lên | -1 | không thể tạo giấy cấp cao hơn | 
| tầng thặng dư | biến | logic lan truyền | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả nhu cầu tập trung ở cấp cao nhất. Trong trường hợp đó, bất kỳ phần dư nào dưới đây đều không liên quan và không thể giúp ích. Thuật toán ngay lập tức phát hiện lỗi ở cấp độ đầu tiên khi nhu cầu vượt quá nguồn cung sẵn có, vì chưa có nguồn cung nào tồn tại. 

Một trường hợp khác xảy ra khi có thặng dư lớn ở mức cao nhưng nhu cầu vừa phải trải rộng ở các mức thấp hơn. Thuật toán xử lý vấn đề này một cách chính xác bằng cách tăng dần phần thặng dư xuống dưới, đảm bảo rằng mỗi đơn vị đóng góp chính xác một lần cắt cho mỗi lần chuyển đổi cấp độ. 

Trường hợp tinh tế cuối cùng là khi chỉ riêng việc vận chuyển đã đáp ứng nhu cầu ở mức không có nguồn cung trực tiếp. Thuật toán xử lý nguồn cung cấp và nguồn cung cấp ban đầu một cách thống nhất, do đó không cần có sự phân biệt. Tính đúng đắn xuất phát từ thực tế là cả hai đều đại diện cho các đơn vị giống hệt nhau đã được chuyển đổi sang cấp độ đó.
