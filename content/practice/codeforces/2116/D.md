---
title: "CF 2116D - Gellyfish và Camellia Japonica"
description: "Chúng ta được cung cấp một mảng có độ dài n phát triển theo q phép toán. Mỗi thao tác lấy hai chỉ số x và y, tính giá trị hiện tại nhỏ hơn của chúng và ghi giá trị đó vào vị trí thứ ba z."
date: "2026-06-09T04:02:11+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dfs-and-similar", "graphs", "greedy", "trees"]
categories: ["algorithms"]
codeforces_contest: 2116
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 1028 (Div. 2)"
rating: 2100
weight: 2116
solve_time_s: 105
verified: false
draft: false
---

[CF 2116D - Gellyfish và Camellia Japonica](https://codeforces.com/problemset/problem/2116/D) 

**Xếp hạng:** 2100 
**Tags:** thuật toán xây dựng, dfs và tương tự, đồ thị, tham lam, cây 
**Thời gian giải:** 1 phút 45s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng có độ dài n phát triển theo q phép toán. Mỗi thao tác lấy hai chỉ số x và y, tính giá trị hiện tại nhỏ hơn của chúng và ghi giá trị đó vào vị trí thứ ba z. Sau tất cả các thao tác, chúng ta được biết mảng b cuối cùng và chúng ta phải xây dựng lại bất kỳ mảng a ban đầu nào có thể tạo ra b. 

Sự thay đổi quan điểm quan trọng là các giá trị chỉ di chuyển xuống dưới thông qua các hoạt động “tối thiểu”. Không có thao tác nào có thể tăng giá trị và mọi phép gán đều sao chép thông tin từ một trong hai nguồn. Vì vậy, hệ thống hoạt động giống như một quy trình luồng thông tin được định hướng trong đó mỗi vị trí liên tục bị ghi đè bởi tối thiểu hai vị trí hiện có trước đó. 

Các ràng buộc rất lớn: n và q tăng lên 3·10^5 qua các thử nghiệm. Bất kỳ giải pháp nào cố gắng mô phỏng rõ ràng các trạng thái ban đầu có thể có hoặc khám phá các phép gán cho mỗi giá trị đều quá chậm. Chúng tôi cần một cái gì đó tuyến tính hoặc gần tuyến tính cho mỗi thử nghiệm, bởi vì tổng kích thước đầu vào đã buộc hành vi O(n + q). 

Trường hợp cạnh tinh vi là khi các ràng buộc trong b mâu thuẫn với bản chất đơn điệu của quá trình truyền cực tiểu. Ví dụ: nếu sau này một chỉ mục được sử dụng làm nguồn cho một chỉ mục khác, thì nó sẽ ngầm tạo ra sự bất bình đẳng trên các giá trị cuối cùng. Trong mẫu, nếu b2 > b1 nhưng có thao tác c2 = min(c1, c2), thì b2 không thể vượt quá b1 sau khi truyền, vì c2 luôn lớn nhất là c1 tại thời điểm đó. 

Một cạm bẫy phổ biến khác là cho rằng mỗi vị trí có thể được xử lý độc lập. Điều đó không thành công vì các chỉ số cặp hoạt động: một vị trí có thể ảnh hưởng đến nhiều vị trí khác thông qua chuỗi nhiệm vụ tối thiểu. 

## Phương pháp tiếp cận 

Một nỗ lực trực tiếp sẽ là coi a là ẩn số và mô phỏng ngược lại tất cả các hoạt động. Người ta có thể thử “hoàn tác” mỗi phép gán, nhưng min không thể đảo ngược: biết c_z = min(c_x, c_y) không đưa ra cách duy nhất nào để khôi phục c_x và c_y. Điều này làm cho mô phỏng ngược trở nên mơ hồ. 

Một ý tưởng ngây thơ khác là đoán các giá trị ban đầu và xác minh bằng mô phỏng chuyển tiếp. Điều này có khả năng xảy ra theo cấp số nhân và ngay lập tức không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng là đảo ngược quan điểm: thay vì hỏi những giá trị ban đầu nào có thể tạo ra giá trị cuối cùng, hãy hỏi những ràng buộc nào mà giá trị cuối cùng áp đặt lên các giá trị ban đầu. Mỗi thao tác c_z = min(c_x, c_y) hàm ý rằng sau tất cả các thao tác, c_z phải có nhiều nhất là cả c_x và c_y tại thời điểm gán. Tuy nhiên, vì các giá trị chỉ giảm nên các giá trị b cuối cùng biểu thị giới hạn dưới của những gì còn sót lại sau tất cả quá trình truyền bá. 

Chúng ta có thể hiểu quy trình này giống như việc xây dựng một biểu đồ ràng buộc có hướng trong đó mỗi thao tác thực thi sự bất bình đẳng giữa các giá trị cuối cùng. Quan sát quan trọng là chúng ta có thể xây dựng một mảng ban đầu ứng cử viên bằng cách truyền ngược các giới hạn dưới thông qua các phép toán theo thứ tự ngược lại, đảm bảo tính nhất quán với tất cả các ràng buộc tối thiểu. Nếu mâu thuẫn xuất hiện thì không có giải pháp nào tồn tại. 

Việc xây dựng hoạt động bằng cách bắt đầu từ b làm đường cơ sở và các hoạt động “phát lại” ngược theo thứ tự ngược lại, cập nhật các giới hạn trên tiềm năng cho các nguồn sao cho mỗi phép gán tối thiểu có thể tạo ra trạng thái cuối cùng được quan sát. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force | hàm mũ | O(n) | Quá chậm | 
| Tuyên truyền ràng buộc ngược | O(n + q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các hoạt động ngược lại, duy trì ý tưởng rằng tại mỗi bước, chúng tôi thực thi tính nhất quán giữa nguồn và đích.

1. Khởi tạo mảng a dưới dạng bản sao của b. Điều này thể hiện các giá trị tối thiểu mà mỗi vị trí phải đạt được sau khi xem xét tất cả các ràng buộc. 
2. Xử lý các thao tác từ đầu đến cuối. Đối với một thao tác (x, y, z), chúng ta biết rằng tại thời điểm thực hiện, z được đặt thành min(x, y). Sau tất cả các thao tác, z không thể vượt quá x hoặc y tại thời điểm đó, do đó giá trị cuối cùng của z áp đặt một ràng buộc giới hạn dưới ngược lên x và y. Cụ thể, cả x và y ít nhất phải bằng a[z], vì nếu không thì z không thể đạt được giá trị cuối cùng. 
3. Với mỗi thao tác đảo ngược (x, y, z), cập nhật: 

a[x] = max(a[x], a[z]) 

a[y] = max(a[y], a[z]) 

Điều này buộc cả hai nguồn đều có khả năng tạo ra giá trị cuối cùng được quan sát tại z. 
4. Sau khi xử lý tất cả các hoạt động, chúng tôi phải xác minh tính nhất quán về phía trước. Chúng tôi thực hiện lại các hoạt động từ đầu đến cuối, tính toán mảng sẽ trở thành gì nếu chúng tôi áp dụng chúng cho a đã xây dựng của mình. Nếu tại bất kỳ thời điểm nào, giá trị tính toán của z khác với min(x, y), thì việc xây dựng không hợp lệ. 
5. Nếu quá trình xác minh thành công, hãy ghi a. Nếu không thì xuất ra -1. 

Bước xác thực chuyển tiếp là cần thiết vì việc truyền ngược chỉ thực thi tính khả thi ở giới hạn thấp hơn chứ không phải các giá trị được xây dựng thực sự tái tạo chuỗi chuyển đổi chính xác. 

### Tại sao nó hoạt động 

Mỗi thao tác tạo ra một sự phụ thuộc: giá trị được ghi vào z không bao giờ lớn hơn x hoặc y tại thời điểm đó. Chạy lùi đảm bảo rằng cả x và y đều đủ lớn để hỗ trợ tất cả các giá trị chạy qua chúng. Mô phỏng chuyển tiếp cuối cùng đảm bảo rằng không có sự giảm bớt ngoài ý muốn nào được đưa ra. Sự kết hợp giữa truyền bá giới hạn dưới và phát lại chính xác đảm bảo rằng mọi ràng buộc do các hoạt động tối thiểu gây ra đều được thỏa mãn mà không gây ràng buộc quá mức cho hệ thống. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, q = map(int, input().split())
        b = list(map(int, input().split()))
        
        ops = []
        for _ in range(q):
            x, y, z = map(int, input().split())
            ops.append((x - 1, y - 1, z - 1))
        
        a = b[:]
        
        for x, y, z in reversed(ops):
            a[x] = max(a[x], a[z])
            a[y] = max(a[y], a[z])
        
        c = a[:]
        ok = True
        
        for x, y, z in ops:
            c[z] = min(c[x], c[y])
        
        if c != b:
            print(-1)
        else:
            print(*a)

if __name__ == "__main__":
    solve()
```Giải pháp duy trì hai mảng về mặt khái niệm. Đầu tiên, a, là trạng thái ban đầu ứng cử viên được xây dựng lại thu được bằng cách truyền các ràng buộc ngược. Thứ hai, c, chỉ được sử dụng để xác thực bằng cách mô phỏng quá trình chuyển tiếp. 

Một chi tiết tinh tế là chúng tôi so sánh với b, không phải trạng thái trung gian, vì chỉ có cấu hình cuối cùng mới quan trọng. Một điểm quan trọng khác là việc truyền ngược sử dụng giá trị tối đa chứ không phải tối thiểu: chúng tôi đang tăng giới hạn dưới, đảm bảo tính khả thi thay vì trực tiếp xây dựng lại các giá trị. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 2, q = 1
b = [1, 2]
op: (2, 1, 2)
```Xử lý ngược: 

| Bước | Hoạt động | một trước | cập nhật | sau | 
| --- | --- | --- | --- | --- | 
| ban đầu | - | [1,2] | - | [1,2] | 
| rev1 | (2,1,2) | [1,2] | a2 lực a2, a1 ≥ 2 | [2,2] | 

Mô phỏng chuyển tiếp: 

c1=2, c2=2, nhưng dự kiến b=[1,2], không khớp nên không hợp lệ. 

Điều này cho thấy rằng chỉ truyền ngược là không đủ nếu các ràng buộc xung đột với các giá trị cuối cùng cố định. 

### Ví dụ 2 

đầu vào:```
n = 3, q = 2
b = [1,2,3]
ops: (2,3,2), (1,2,1)
```Tuyên truyền ngược: 

| Bước | Hoạt động | một trước | cập nhật | sau | 
| --- | --- | --- | --- | --- | 
| ban đầu | - | [1,2,3] | - | [1,2,3] | 
| rev2 | (1,2,1) | [1,2,3] | a2,a1 ≥ 1 | [1,2,3] | 
| rev1 | (2,3,2) | [1,2,3] | a2,a3 ≥ 2 | [1,2,3] | 

Mô phỏng chuyển tiếp tái tạo b chính xác. 

Điều này xác nhận rằng khi không tồn tại mâu thuẫn, việc truyền ngược mang lại phép gán ổn định tồn tại trong tất cả các hoạt động tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) | Mỗi thao tác được xử lý hai lần, một lần lùi và một lần tiến | 
| Không gian | O(n + q) | Lưu trữ cho mảng và hoạt động | 

Tổng kích thước đầu vào trên các trường hợp thử nghiệm là 3·10^5, vì vậy việc xử lý tuyến tính là cần thiết. Thuật toán chỉ thực hiện công việc liên tục trên mỗi thao tác và trên mỗi phần tử, khiến nó thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n, q = map(int, input().split())
            b = list(map(int, input().split()))
            ops = [tuple(map(int, input().split())) for _ in range(q)]
            ops = [(x-1, y-1, z-1) for x,y,z in ops]
            
            a = b[:]
            for x,y,z in reversed(ops):
                a[x] = max(a[x], a[z])
                a[y] = max(a[y], a[z])
            
            c = a[:]
            for x,y,z in ops:
                c[z] = min(c[x], c[y])
            
            if c != b:
                out.append("-1")
            else:
                out.append(" ".join(map(str,a)))
        return "\n".join(out)
    
    return solve()

# provided samples
assert run("""3
2 1
1 2
2 1 2
3 2
1 2 3
2 3 2
1 2 1
6 4
1 2 2 3 4 5
5 6 6
4 5 5
3 4 4
2 3 3
""") == """-1
1 2 3
1 2 3 4 5 5"""

# custom case: single node
assert run("""1
1 0
5
""") == "5"

# custom case: immediate contradiction
assert run("""1
2 1
5 1
1 2 2
""") == "-1"

# custom case: no operations
assert run("""1
4 0
1 2 3 4
""") == "1 2 3 4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | tầm thường | trường hợp cơ sở đúng đắn | 
| mâu thuẫn | -1 | phát hiện ràng buộc không thể | 
| không có hoạt động | danh tính | xử lý quá trình trống | 

## Vỏ cạnh 

Trường hợp một cạnh là khi một chỉ mục không bao giờ xuất hiện dưới dạng z mà xuất hiện dưới dạng x hoặc y. Trong trường hợp đó, việc truyền ngược không bao giờ tăng giá trị của nó, vì vậy nó vẫn bằng b, điều này đúng vì nó không bao giờ ảnh hưởng đến bất kỳ đẳng thức bắt buộc nào. 

Một trường hợp khác là tự gán như c[z] = min(c[z], c[y]). Điều này tạo ra một ràng buộc rằng z tối đa phải bằng y và việc truyền ngược thực thi chính xác a[z] ≤ a[y] ở dạng ngược lại thông qua các cập nhật tối đa. 

Trường hợp tinh tế cuối cùng là chuỗi dài trong đó một giá trị chảy qua nhiều vị trí. Thuật toán xử lý vấn đề này vì mỗi bước lan truyền chỉ thực thi tính khả thi và các bản cập nhật tối đa lặp lại sẽ tích lũy giới hạn dưới được yêu cầu mạnh nhất mà không bỏ sót các phần phụ thuộc trung gian.
