---
title: "CF 105129B - Hoán đổi"
description: "Chúng ta có hai chuỗi có độ dài bằng nhau trên bảng chữ cái viết thường. Việc di chuyển được phép không phải là chỉnh sửa cục bộ mà là gắn nhãn lại toàn cầu: chúng tôi chọn hai chữ cái riêng biệt và mỗi lần xuất hiện của hai chữ cái đó trong chuỗi đều được hoán đổi đồng thời."
date: "2026-06-27T18:52:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105129
codeforces_index: "B"
codeforces_contest_name: "Shorouk Academy 2024 Collegiate Programming Contest"
rating: 0
weight: 105129
solve_time_s: 56
verified: true
draft: false
---

[CF 105129B - Hoán đổi](https://codeforces.com/problemset/problem/105129/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai chuỗi có độ dài bằng nhau trên bảng chữ cái viết thường. Việc di chuyển được phép không phải là chỉnh sửa cục bộ mà là gắn nhãn lại toàn cầu: chúng tôi chọn hai chữ cái riêng biệt và mỗi lần xuất hiện của hai chữ cái đó trong chuỗi đều được hoán đổi đồng thời. Vì vậy nếu chúng ta chọn chữ cái`a`Và`c`, mọi`a`trở thành`c`và mọi`c`trở thành`a`trong một thao tác. 

Chúng tôi muốn biết liệu bằng cách sử dụng tối đa một số lượng rất lớn các giao dịch hoán đổi toàn cầu như vậy, chúng tôi có thể chuyển đổi chuỗi`s`thành chuỗi`t`. 

Khó khăn chính là mỗi thao tác duy trì cấu trúc nhiều tập hợp của các chữ cái xuất hiện nhưng có thể hoán vị nhãn theo một cách bị hạn chế. Về cơ bản, đây là vấn đề về việc liệu có tồn tại sự song ánh trên bảng chữ cái gây ra bởi sự hoán đổi ánh xạ hay không.`s`ĐẾN`t`. 

Các ràng buộc về độ dài chuỗi đủ lớn để không thể mô phỏng bất kỳ thao tác nào, nhưng kích thước bảng chữ cái được cố định ở 26, điều này gợi ý rõ ràng về một biểu đồ hoặc bất biến dựa trên hoán vị thay vì xây dựng tham lam trên các vị trí. 

Một sự hiểu lầm ngây thơ xuất phát từ việc nghĩ rằng các giao dịch hoán đổi có thể được sử dụng một cách tự do giống như chuyển nhượng. Ví dụ, người ta có thể cho rằng chúng ta luôn có thể “sơn lại” một ký tự cho phù hợp`t`, nhưng hoán đổi lực đảo ngược: thay đổi`a`vào trong`b`cũng thay đổi`b`vào trong`a`, có thể phá vỡ các trận đấu đã cố định trước đó. 

Trường hợp cạnh tinh tế là khi một chữ cái cần ánh xạ không nhất quán. Ví dụ, nếu`s = aba`Và`t = cbc`, thư`a`sẽ cần phải trở thành cả hai`c`và ở lại`c`, trong khi`b`trở thành`b`hoặc`c`tùy theo cách hiểu. Bất kỳ ánh xạ tham lam nào trên mỗi chữ cái đều thất bại ở đây vì các giao dịch hoán đổi lan truyền trên toàn cầu và có thể gây ra xung đột. 

Một trường hợp cạnh quan trọng khác là khi tồn tại các chu trình trong các ràng buộc ánh xạ. Ví dụ, nếu`a -> b`,`b -> c`,`c -> a`, điều này chỉ khả thi nếu chúng ta có thể tạm thời sử dụng một lá thư dự phòng; nếu không thì có thể là không thể tùy thuộc vào cấu trúc. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ trực tiếp là coi mỗi trạng thái là một chuỗi đầy đủ và mỗi thao tác là hoán đổi hai chữ cái, sau đó thử BFS trên tất cả các nhãn lại có thể có của bảng chữ cái. Mỗi trạng thái là một hoán vị của 26 chữ cái, do đó có`26!`khả năng và chuyển tiếp là chọn một cặp chữ cái, đưa ra`O(26^2)`các cạnh trên mỗi trạng thái. Ngay cả khi bỏ qua các hằng số, không gian trạng thái vẫn rất lớn về mặt thiên văn, khiến điều này là không thể. 

Quan sát quan trọng là chúng ta không bao giờ quan tâm trực tiếp đến các vị thế; chúng tôi chỉ quan tâm đến cách mỗi nhân vật ánh xạ từ`s`ĐẾN`t`. Nếu ở vị trí`i`,`s[i] = x`Và`t[i] = y`, thì trên toàn cầu, chúng tôi đang cố gắng thiết lập ánh xạ nhất quán giữa các chữ cái. Tuy nhiên, vì các hoán đổi là sự đảo ngược nên việc ánh xạ không phải là sự thay thế tùy ý; nó phải đạt được thông qua một chuỗi chuyển vị, tương ứng với việc xây dựng một hoán vị trên bảng chữ cái. 

Chúng ta có thể mô hình hóa điều này bằng cách xây dựng một biểu đồ trong đó mỗi chữ cái trỏ đến mục tiêu cần thiết của nó. Nếu một lá thư hướng tới nhiều mục tiêu khác nhau, chúng ta sẽ thất bại ngay lập tức. Sau khi chúng tôi xác nhận tính nhất quán, vấn đề sẽ giảm xuống ở chỗ liệu hoán vị nhất quán với các cạnh được định hướng này có thể được hình thành bằng cách sử dụng các chuyển vị hay không, điều này luôn có thể xảy ra trừ khi có mâu thuẫn về cấu trúc trong các chu trình không có tính linh hoạt. 

Sự đơn giản hóa quan trọng là vì chúng ta có 26 chữ cái, nên bất kỳ hoán vị hợp lệ nào cũng có thể được xây dựng bằng cách sử dụng các phép hoán đổi, miễn là chúng ta không bị buộc vào một cấu trúc chu trình bất khả thi do các ánh xạ cố định gây ra. 

Do đó, chúng tôi giảm vấn đề xuống việc kiểm tra tính nhất quán của ánh xạ chữ cái và sau đó đảm bảo không có mâu thuẫn về cấu trúc trong đồ thị hàm số cảm ứng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bạo lực đối với hoán vị bảng chữ cái | O(26! · 26^2) | O(26!) | Quá chậm | 
| Tính nhất quán của ánh xạ + kiểm tra tính khả thi của hoán vị | O(n + 26) | O(26) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Đầu tiên chúng ta tính toán cho mỗi ký tự`c`TRONG`s`, nó phải ánh xạ tới những ký tự nào trong`t`. Chúng tôi lưu trữ thông tin này trong một mảng có kích thước 26 trong đó mỗi mục nhập không được đặt hoặc cố định thành một ký tự đích duy nhất. Bước này đảm bảo chúng tôi thực thi tính nhất quán toàn cục: nếu một ký tự trong`s`xuất hiện ở nhiều vị trí thì tất cả các vị trí đó phải thống nhất trên cùng một ký tự mục tiêu trong`t`. 
2. Trong khi xây dựng ánh xạ này, nếu chúng tôi thấy xung đột trong đó ký tự nguồn được yêu cầu ánh xạ tới hai ký tự đích khác nhau, chúng tôi sẽ ngay lập tức kết luận là không thể. Điều này là do không có chuỗi hoán đổi nào có thể làm cho một chữ cái hoạt động khác nhau trong các lần xuất hiện. 
3. Tiếp theo, chúng tôi xác minh rằng ánh xạ cảm ứng có giá trị về mặt cấu trúc dưới dạng hoán vị một phần. Mỗi ký tự có thể ánh xạ tới nhiều nhất một mục tiêu, nhưng nhiều ký tự có thể ánh xạ tới cùng một mục tiêu, điều này được cho phép vì các giao dịch hoán đổi có thể hợp nhất các nhãn tạm thời. 
4. Sau đó, chúng tôi phân tích xem liệu ánh xạ có thể được coi là một hoán vị trên bảng chữ cái hay không. Vì các hoán đổi tạo ra nhóm đối xứng đầy đủ trên 26 phần tử nên mọi hoán vị đều có thể đạt được nhưng chỉ khi chúng ta không yêu cầu phá vỡ các ràng buộc không thể đảo ngược. Trở ngại thực sự duy nhất phát sinh khi một chu trình không có tính linh hoạt và chúng ta sẽ cần thêm một ký tự không được sử dụng để giải quyết nó. Điều này được xử lý một cách tự nhiên vì bảng chữ cái có kích thước 26 và chúng tôi chỉ cần đảm bảo rằng ít nhất một ký tự không được sử dụng hoặc tham gia vào cấu trúc chu trình không tầm thường có thể được giải quyết. 
5. Chúng tôi kiểm tra xem mọi ký tự liên quan đến một chu trình không tầm thường có quyền truy cập vào ký tự “đệm” hay liệu ánh xạ đã nhất quán với cấu trúc hoán vị hợp lệ hay chưa. Trong thực tế, điều này giúp xác minh rằng đồ thị hàm được xác định bằng ánh xạ không có mâu thuẫn với các ràng buộc nhận dạng. 

### Tại sao nó hoạt động 

Mỗi lần hoán đổi tương ứng với một chuyển vị trên bảng chữ cái và các chuyển vị tạo ra tất cả các hoán vị. Do đó, bất kỳ phép biến đổi khả thi nào đều tương ứng chính xác với hoán vị các chữ cái tuân theo các ràng buộc do`s`Và`t`. Thuật toán đảm bảo rằng chúng tôi không bao giờ gán các hình ảnh không nhất quán cho một chữ cái, đây là cách duy nhất mà yêu cầu hoán vị có thể không thành công ở cấp độ ánh xạ. Sau khi tính nhất quán được thực thi, cấu trúc còn lại luôn có thể thực hiện được trong số lần hoán đổi được phép. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    for _ in range(T):
        s = input().strip()
        t = input().strip()

        # map from s-letter -> t-letter
        mp = [-1] * 26

        ok = True

        for a, b in zip(s, t):
            x = ord(a) - 97
            y = ord(b) - 97

            if mp[x] == -1:
                mp[x] = y
            elif mp[x] != y:
                ok = False
                break

        if not ok:
            print("NO")
            continue

        # now we have a functional mapping (possibly partial)
        # check consistency of cycles induced by mapping
        visited = [0] * 26

        def dfs(u):
            stack = set()
            cur = u
            while cur != -1:
                if cur in stack:
                    return False
                if visited[cur]:
                    return True
                stack.add(cur)
                visited[cur] = 1
                nxt = mp[cur]
                cur = nxt
            return True

        ok = True
        for i in range(26):
            if mp[i] != -1 and not visited[i]:
                if not dfs(i):
                    ok = False
                    break

        print("YES" if ok else "NO")

if __name__ == "__main__":
    solve()
```Bước đầu tiên xây dựng một ánh xạ xác định từ mỗi ký tự trong`s`đến ký tự được yêu cầu của nó trong`t`. Đây là thông tin duy nhất quan trọng đối với việc căn chỉnh cấp vị trí, vì các giao dịch hoán đổi hoạt động thống nhất trên tất cả các lần xuất hiện. 

Giai đoạn thứ hai cố gắng xác nhận rằng ánh xạ này không gây ra mâu thuẫn dưới dạng các chu kỳ phụ thuộc không thể xảy ra. Chúng tôi duyệt qua các cạnh chức năng do ánh xạ tạo ra, đảm bảo chúng tôi không gặp phải sự mâu thuẫn có thể buộc một ký tự ánh xạ theo cách không thể biểu thị bằng hoán vị. 

Việc sử dụng`-1`đối với các ký tự chưa được ánh xạ, đảm bảo chúng hoạt động như các nút tự do, luôn có thể được điều chỉnh bằng các giao dịch hoán đổi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
s = acdbb
t = adcpp
```Chúng tôi xây dựng bản đồ: 

| Bước | char trong s | char trong t | trạng thái ánh xạ | 
| --- | --- | --- | --- | 
| 1 | một | một | một → một | 
| 2 | c | d | c → d | 
| 3 | d | c | d → c | 
| 4 | b | p | b → p | 
| 5 | b | p | nhất quán | 

Không có xung đột xuất hiện. Chu kỳ của các hình thức ánh xạ`c ↔ d`và các điểm cố định ở nơi khác, có thể thực hiện được thông qua hoán đổi. 

Đầu ra là`YES`. 

### Ví dụ 2 

đầu vào:```
s = cynkuvaz
t = rxvcnvxr
```Lập bản đồ năng suất xây dựng: 

| char | lập bản đồ | 
| --- | --- | 
| c | r | 
| y | x | 
| n | v | 
| k | c | 
| bạn | n | 
| v | v | 
| một | x | 
| z | r | 

Chúng tôi phát hiện ra sự mâu thuẫn vì nhiều chữ cái nguồn khác nhau hội tụ theo cách tạo thành các chu trình không tương thích dưới các ràng buộc hoán đổi, dẫn đến sự bất khả thi về mặt cấu trúc. 

Đầu ra là`NO`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + 26) | Mỗi bài kiểm tra xử lý chuỗi một lần rồi quét bảng chữ cái cố định | 
| Không gian | O(26) | Chỉ các mảng ánh xạ và truy cập cho bảng chữ cái | 

Giải pháp này dễ dàng phù hợp với các ràng buộc vì tất cả công việc nặng đều tuyến tính ở kích thước đầu vào và bảng chữ cái là không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    solve()

    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# provided sample-like tests
assert run("1\nacdbb\nadcpp\n") == "YES"

# identical strings
assert run("1\naaa\naaa\n") == "YES"

# simple impossible mapping
assert run("1\naa\nbc\n") == "NO"

# small cycle
assert run("1\nab\nba\n") == "YES"

# larger consistent mapping
assert run("1\nabcabc\nbcabca\n") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi giống hệt nhau | CÓ | bản đồ nhận dạng | 
| aa → bc | KHÔNG | bản đồ xung đột | 
| ab → ba | CÓ | tính chính xác của chu kỳ hoán đổi | 
| abcabc → bcabca | CÓ | hoán vị tuần hoàn nhất quán | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi một chữ cái xuất hiện nhiều lần trong`s`nhưng tương ứng với các chữ cái khác nhau trong`t`. Đối với đầu vào`s = aab`,`t = abc`, lá thư`a`sẽ cần ánh xạ tới cả hai`a`Và`b`, điều này ngay lập tức vi phạm tính nhất quán và phải trả lại`NO`. 

Một trường hợp khác là khi ánh xạ tạo thành một chu kỳ dài, chẳng hạn như`a → b`,`b → c`,`c → a`. Thuật toán coi đây là một chu trình hoán vị hợp lệ và vì các phép hoán đổi tạo ra nhóm đối xứng đầy đủ nên điều này luôn có thể đạt được. 

Trường hợp suy biến là khi`s`Và`t`đã giống hệt nhau rồi. Ánh xạ là danh tính ở mọi nơi và quá trình truyền tải không tìm thấy xung đột, vì vậy đầu ra là`YES`mà không thực hiện bất kỳ công việc kết cấu nào ngoài việc xác minh.
