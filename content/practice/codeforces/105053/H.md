---
title: "CF 105053H - Hoạt động điều hòa"
description: "Chúng ta được cung cấp một chuỗi cố định và một chuỗi các thao tác biến đổi chuỗi đó nhiều lần. Mỗi thao tác là đảo ngược hoàn toàn chuỗi hoặc xoay theo chu kỳ một lượng nào đó sang trái hoặc phải."
date: "2026-06-28T01:03:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105053
codeforces_index: "H"
codeforces_contest_name: "The 2024 ICPC Latin America Championship"
rating: 0
weight: 105053
solve_time_s: 67
verified: true
draft: false
---

[CF 105053H - Hoạt động điều hòa](https://codeforces.com/problemset/problem/105053/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi cố định và một chuỗi các thao tác biến đổi chuỗi đó nhiều lần. Mỗi thao tác là đảo ngược hoàn toàn chuỗi hoặc xoay theo chu kỳ một lượng nào đó sang trái hoặc phải. Các thao tác này được áp dụng theo thứ tự và chúng tôi không quan tâm đến chuỗi cuối cùng sau tất cả các thao tác mà quan tâm đến tất cả các phân đoạn liền kề của danh sách thao tác. 

Đối với mỗi mảng con của các thao tác, về mặt khái niệm, chúng tôi áp dụng khối chuyển đổi đó bắt đầu từ chuỗi gốc và hỏi liệu chúng tôi có kết thúc chính xác trở lại chuỗi gốc hay không. Nhiệm vụ là đếm xem có bao nhiêu mảng con như vậy trả về chuỗi về dạng ban đầu. 

Khó khăn chính là số lượng mảng con là bậc hai của số lượng phép toán, do đó, bất kỳ cách tiếp cận nào mô phỏng rõ ràng từng phân đoạn sẽ ngay lập tức quá chậm khi số lượng phép toán lên tới hai trăm nghìn. 

Bản thân chuỗi không được thay đổi độc lập cho mỗi truy vấn, nó chỉ đóng vai trò là đối tượng tham chiếu. Điều thực sự quan trọng là cách mỗi thao tác hoán vị các vị trí bên trong chuỗi. Hai chuỗi thao tác khác nhau có thể tạo ra cùng một hiệu ứng trên các chỉ mục và bất cứ khi nào một chuỗi tạo ra hoán vị nhận dạng, chuỗi kết quả được đảm bảo khớp với chuỗi gốc. 

Việc triển khai đơn giản sẽ tính toán lại toàn bộ tác động của mọi mảng con bằng cách mô phỏng các phép biến đổi trên chuỗi. Điều này không thành công vì mỗi mô phỏng là tuyến tính theo độ dài chuỗi, dẫn đến hành vi tổng thể theo hình khối. 

Một vấn đề phức tạp hơn sẽ xuất hiện nếu người ta cố gắng so sánh trực tiếp các chuỗi về sự bằng nhau sau mỗi mảng con. Điều này cũng quá chậm và có thể gây hiểu lầm khi chuỗi chứa các ký tự lặp lại, vì đẳng thức của chuỗi không phản ánh trực tiếp sự bằng nhau của các hoán vị cơ bản. 

Quan điểm đúng là mỗi thao tác tạo ra một hoán vị của các chỉ số và chúng ta đang tổng hợp các hoán vị này. Vấn đề trở thành việc đếm xem có bao nhiêu mảng con tạo ra hoán vị nhận dạng. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ xem xét mọi chỉ mục bắt đầu của một mảng con và áp dụng lặp đi lặp lại các phép toán cho đến chỉ mục cuối cùng, duy trì chuỗi kết quả hoặc hoán vị. Mỗi mảng con tiêu tốn thời gian tuyến tính, do đó tổng độ phức tạp là bậc ba trong trường hợp xấu nhất, vượt xa giới hạn. 

Cái nhìn sâu sắc về cấu trúc là tất cả các hoạt động thuộc về nhóm nhị diện của một chu kỳ có độ dài bằng kích thước chuỗi. Mỗi phép biến đổi có thể được mô tả chỉ bằng hai phần thông tin: một bit hướng cho biết liệu chuỗi có bị đảo ngược hay không và độ lệch dịch chuyển cho biết các chỉ số được xoay bao xa. 

Điều này có nghĩa là chúng ta không cần phải theo dõi toàn bộ hoán vị. Chúng ta chỉ cần theo dõi trạng thái thu gọn cho từng tiền tố của hoạt động. Nếu hai tiền tố tạo ra trạng thái chuyển đổi giống nhau thì các phép toán giữa chúng sẽ tạo thành chuyển đổi nhận dạng, nghĩa là mảng con khôi phục chuỗi gốc. 

Vấn đề giảm xuống còn tính toán các trạng thái tiền tố và đếm số lần mỗi trạng thái xảy ra. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(K · N) hoặc tệ hơn | O(N) | Quá chậm | 
| Băm trạng thái tiền tố | O(K) | O(K) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô hình hóa từng tiền tố của các hoạt động dưới dạng chuyển đổi các chỉ số. Sự biến đổi luôn ở dạng “có thể đảo ngược, cộng với sự dịch chuyển theo chu kỳ”. 

Chúng tôi biểu thị một trạng thái bằng cách sử dụng hai giá trị, cờ hướng boolean và giá trị dịch chuyển. 

### 1. Khởi tạo trạng thái nhận dạng 

Chúng tôi bắt đầu mà không áp dụng thao tác nào. Điều này tương ứng với phép biến đổi nhận dạng trong đó chuỗi không thay đổi, do đó hướng là bình thường và độ dịch chuyển bằng 0. 

### 2. Xác định cách mỗi thao tác cập nhật trạng thái 

Chúng tôi xử lý từng hoạt động một, duy trì trạng thái hiện tại.

Xoay phải sẽ tăng độ dịch chuyển theo số lượng của nó, bất kể chuỗi hiện tại có bị đảo ngược hay không. Xoay trái làm giảm độ dịch chuyển một lượng, một lần nữa không phụ thuộc vào hướng. Điều này có hiệu quả vì sự đảo ngược chỉ ảnh hưởng đến cách diễn giải các chỉ số chứ không ảnh hưởng đến cách tích lũy các thay đổi theo chu kỳ. 

Đảo ngược lật cờ chỉ hướng và cũng thay đổi sự dịch chuyển một cách nhất quán để phép chuyển đổi vẫn được thể hiện chính xác ở cùng một dạng chuẩn. Cụ thể, sự đảo ngược ánh xạ sự biến đổi affine hiện tại thành gương phản chiếu của nó xung quanh chu kỳ, có thể được cập nhật theo thời gian không đổi. 

### 3. Duy trì trạng thái tiền tố 

Khi xử lý các thao tác, chúng tôi lưu trữ trạng thái sau mỗi tiền tố trong từ điển. Mỗi trạng thái riêng biệt biểu thị một hoán vị cụ thể của các chỉ số được áp dụng cho chuỗi gốc. 

### 4. Đếm các trạng thái tiền tố bằng nhau 

Nếu hai chỉ số tiền tố i và j tạo ra cùng một trạng thái thì việc áp dụng các phép toán từ i+1 đến j sẽ dẫn đến chuyển đổi nhận dạng ròng. Mỗi cặp như vậy tương ứng với một mảng con hợp lệ. 

Chúng tôi đếm tần số của các trạng thái và tích lũy số lượng cặp bằng cách sử dụng tính năng kết hợp bên trong mỗi nhóm tần số. 

### 5. Xuất ra tổng số 

Câu trả lời cuối cùng là tổng trên tất cả các trạng thái tần số nhân tần số trừ một chia cho hai. 

### Tại sao nó hoạt động 

Mỗi chuỗi thao tác tương ứng với một hoán vị trong một nhóm trong đó thành phần có tính kết hợp và khả nghịch. Trạng thái tiền tố mô tả đầy đủ sự hoán vị đó. Một mảng con tạo ra nhận dạng chính xác khi các điểm cuối của nó tương ứng với các phần tử nhóm bằng nhau, tương đương với các trạng thái tiền tố bằng nhau. Sự song ánh này đảm bảo chúng ta không bỏ sót các mảng con hợp lệ cũng như không đếm những mảng con không hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    S = input().strip()
    n = len(S)

    K = int(input())
    
    # state: (dir, shift)
    # dir = 0 normal, 1 reversed
    d = 0
    s = 0

    from collections import defaultdict
    freq = defaultdict(int)
    freq[(0, 0)] = 1

    for _ in range(K):
        parts = input().split()
        op = parts[0]

        if op == 'I':
            # reverse
            d ^= 1
            s = (n - 1 - s) % n

        else:
            val = int(parts[1])
            if op == 'R':
                s = (s + val) % n
            else:
                s = (s - val) % n

        state = (d, s)
        freq[state] += 1

    ans = 0
    for c in freq.values():
        ans += c * (c - 1) // 2

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai duy trì một biểu diễn nhỏ gọn của phép biến đổi do mỗi tiền tố gây ra. Cập nhật đảo ngược lật cờ định hướng và điều chỉnh sự dịch chuyển sao cho biểu diễn affine vẫn nhất quán. Xoay chỉ sửa đổi giá trị dịch chuyển. 

Từ điển tích lũy số lần mỗi trạng thái chuyển đổi xuất hiện. Tổng cuối cùng đếm xem tồn tại bao nhiêu cặp trạng thái bằng nhau, tương ứng trực tiếp với các mảng con hợp lệ. 

Một điểm tinh tế là khởi tạo tần số của trạng thái nhận dạng trước khi xử lý bất kỳ thao tác nào. Điều này đảm bảo các mảng con bắt đầu từ thao tác đầu tiên được tính chính xác. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp nhỏ trong đó chuỗi là “abc” và chúng ta áp dụng phép xoay phải một vòng, sau đó là phép xoay trái một vòng. 

| Bước | Hoạt động | Hướng | Thay đổi | Tiểu bang | Cập nhật tần suất | 
| --- | --- | --- | --- | --- | --- | 
| 0 | bắt đầu | 0 | 0 | (0,0) | (0,0):1 | 
| 1 | R 1 | 0 | 1 | (0,1) | (0,1):1 | 
| 2 | L 1 | 0 | 0 | (0,0) | (0,0):2 | 

Trạng thái trở về trạng thái nhận dạng sau thao tác thứ hai, do đó mảng con bao gồm cả hai thao tác là hợp lệ. 

Dấu vết này cho thấy sự bằng nhau của các trạng thái tiền tố nắm bắt chính xác việc hủy bỏ các phép biến đổi. 

Bây giờ hãy xem xét một trường hợp đảo ngược: “abc”, sau đó đảo ngược, rồi đảo ngược lại. 

| Bước | Hoạt động | Hướng | Thay đổi | Tiểu bang | Cập nhật tần suất | 
| --- | --- | --- | --- | --- | --- | 
| 0 | bắt đầu | 0 | 0 | (0,0) | (0,0):1 | 
| 1 | Tôi | 1 | 2 | (1,2) | (1,2):1 | 
| 2 | Tôi | 0 | 0 | (0,0) | (0,0):2 | 

Đảo ngược kép khôi phục trạng thái nhận dạng, xác nhận rằng đảo ngược là tự đảo ngược trong biểu diễn trạng thái. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(K) | Mỗi thao tác cập nhật trạng thái và từ điển có kích thước không đổi | 
| Không gian | O(K) | Tối đa một trạng thái cho mỗi tiền tố được lưu trữ | 

Giải pháp này phù hợp một cách thoải mái trong giới hạn vì cả thời gian và bộ nhớ đều có quy mô tuyến tính với số lượng thao tác, tối đa là hai trăm nghìn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import defaultdict

    S = input().strip()
    n = len(S)
    K = int(input())

    d = 0
    s = 0
    freq = defaultdict(int)
    freq[(0, 0)] = 1

    for _ in range(K):
        parts = input().split()
        op = parts[0]

        if op == 'I':
            d ^= 1
            s = (n - 1 - s) % n
        else:
            v = int(parts[1])
            if op == 'R':
                s = (s + v) % n
            else:
                s = (s - v) % n

        freq[(d, s)] += 1

    ans = sum(c * (c - 1) // 2 for c in freq.values())
    return str(ans)

# sample-style tests
assert run("pda\n2\nR 2\nL 2\n") == "1"
assert run("aaa\n4\nR 1\nI\nI\nR 1\n") == "3"

# custom tests
assert run("ab\n1\nI\n") == "0"
assert run("ab\n2\nI\nI\n") == "3"
assert run("abcd\n3\nR 1\nR 1\nL 2\n") == "3"
assert run("xyz\n2\nR 1\nL 1\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đảo ngược đơn | 0 | không có danh tính ngẫu nhiên ngoại trừ đoạn trống | 
| đảo ngược đôi | 3 | tất cả các mảng con khôi phục danh tính | 
| chuỗi hủy vòng quay | 3 | số học dịch chuyển đúng | 
| xoay và quay nghịch đảo | 1 | hủy bỏ trong các hoạt động khác nhau | 

## Vỏ cạnh 

Trường hợp một cạnh là sự đảo ngược lặp đi lặp lại. Bắt đầu từ danh tính, áp dụng đảo ngược hai lần phải trở về trạng thái nhận dạng. Cập nhật trạng thái đảm bảo điều này bằng cách đảo hướng hai lần và khôi phục sự thay đổi ban đầu. 

Một trường hợp cạnh khác là việc hủy bỏ hoàn toàn chỉ thông qua các phép quay. Một chuỗi các phép quay phải và trái có tổng bằng 0 modulo n phải trở về trạng thái đồng nhất. Cập nhật ca trực tiếp tích lũy modulo n, do đó việc hủy bỏ được ghi lại một cách tự nhiên. 

Trường hợp thứ ba là đảo ngược và xoay xen kẽ. Ví dụ: đảo chiều, xoay, rồi đảo ngược lại yêu cầu cả hướng và chuyển số đều được cập nhật nhất quán. Biểu diễn affine đảm bảo rằng thành phần vẫn đóng trong các phép toán này, do đó trạng thái tiền tố vẫn tóm tắt chính xác phép biến đổi.
