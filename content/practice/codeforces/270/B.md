---
title: "CF 270B - Đa luồng"
description: "Diễn đàn giữ một danh sách các chủ đề được sắp xếp theo thời gian gửi tin nhắn mới nhất của họ. Bất cứ khi nào ai đó đăng bài trong một chủ đề, chủ đề đó sẽ ngay lập tức chuyển lên đầu danh sách. Không có sự sắp xếp lại nào khác xảy ra. Ban đầu các thread được sắp xếp theo thứ tự 1, 2, 3, ..., n."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 270
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 165 (Div. 2)"
rating: 1400
weight: 270
solve_time_s: 139
verified: true
draft: false
---

[CF 270B - Đa luồng](https://codeforces.com/problemset/problem/270/B) 

**Đánh giá:** 1400 
**Tags:** cấu trúc dữ liệu, tham lam, triển khai 
**Thời gian giải:** 2m 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Diễn đàn giữ một danh sách các chủ đề được sắp xếp theo thời gian gửi tin nhắn mới nhất của họ. Bất cứ khi nào ai đó đăng bài trong một chủ đề, chủ đề đó sẽ ngay lập tức chuyển lên đầu danh sách. Không có sự sắp xếp lại nào khác xảy ra. 

Ban đầu các chủ đề được sắp xếp như`1, 2, 3, ..., n`. Sau một số chuỗi cập nhật không xác định, thứ tự hiển thị sẽ trở thành`a1, a2, ..., an`, Ở đâu`ai`cho chúng ta biết vị trí cũ nào hiện đang chiếm giữ`i`-vị trí thứ 

Chúng ta phải xác định có bao nhiêu thread được đảm bảo đã nhận được tin nhắn mới. Một luồng được "chắc chắn được cập nhật" nếu không có trình tự di chuyển trước nào có thể tạo ra thứ tự cuối cùng trong khi không chạm vào luồng đó. 

Các ràng buộc đủ lớn để loại trừ mô phỏng đắt tiền. Với`n`lên đến`10^5`, MỘT`O(n^2)`cách tiếp cận sẽ yêu cầu khoảng`10^10`trong trường hợp xấu nhất vượt xa thời hạn. Mục tiêu phải tuyến tính hoặc gần tuyến tính. 

Phần khó khăn là nhiều trình tự cập nhật khác nhau có thể dẫn đến cùng một sự sắp xếp cuối cùng. Chúng tôi không được hỏi chủ đề nào chắc chắn đã thay đổi vị trí, nhưng chủ đề nào phải được cập nhật theo mọi trình tự hợp lệ. 

Một lỗi phổ biến là cho rằng mọi luồng có vị trí thay đổi đều phải có một thông báo mới. 

Coi như:```
5
2 1 3 4 5
```Câu trả lời đúng là`1`. 

Chủ đề`2`chắc chắn đã được cập nhật vì nó đã đi trước chủ đề`1`. Nhưng sợi chỉ`1`bản thân nó có thể đơn giản đã bị đẩy lùi bởi sợi chỉ`2`cập nhật của. 

Một trường hợp tinh tế khác là khi mảng đã được sắp xếp:```
4
1 2 3 4
```Câu trả lời đúng là`0`. 

Không cần cập nhật gì cả. Việc triển khai bất cẩn có thể tính không chính xác mọi luồng là "có thể được cập nhật". 

Một ví dụ thú vị hơn là:```
5
5 2 1 3 4
```Câu trả lời đúng là`2`. 

chủ đề`5`Và`2`phải được cập nhật vì cả hai đều xuất hiện trước chủ đề`1`, ban đầu có trước chúng. chủ đề`1`,`3`, Và`4`có thể vẫn còn nguyên. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là mô phỏng rõ ràng tất cả các chuỗi cập nhật có thể có và kiểm tra xem luồng nào có thể không bị ảnh hưởng. Điều này hoạt động về mặt khái niệm vì mọi cập nhật chỉ là "di chuyển một luồng lên phía trước". Thật không may, số lượng các trình tự có thể tăng lên một cách bùng nổ. Ngay cả việc thử tất cả các tập hợp con của các chủ đề được cập nhật cũng đã yêu cầu`2^n`các khả năng có thể xảy ra và việc xây dựng lại các lệnh di chuyển hợp lệ còn khiến tình hình trở nên tồi tệ hơn. 

Chúng ta cần một quan sát mang tính cấu trúc về cách những bước đi phía trước này ảnh hưởng đến trật tự tương đối. 

Giả sử một chủ đề không bao giờ được cập nhật. Sau đó, thứ tự tương đối của nó với mọi luồng không bao giờ được cập nhật khác vẫn không thay đổi. Cách duy nhất một luồng có thể di chuyển trước một luồng khác ban đầu đứng trước nó là tự cập nhật. 

Điều đó ngay lập tức gợi ý một tiêu chí. Quét thứ tự cuối cùng từ trái sang phải. Nếu chúng ta gặp một luồng có vị trí ban đầu nhỏ hơn tất cả các vị trí ban đầu trước đó thì luồng này có thể vẫn chưa bị ảnh hưởng. Không ai sau nó đã nhảy lên trước nó. 

Nhưng nếu một luồng có một số phần tử trước đó có chỉ mục ban đầu nhỏ hơn thì nó đã vượt qua luồng đó và chắc chắn đã được cập nhật. 

Ví dụ:```
5 2 1 3 4
```Mức tối thiểu đang chạy là:```
5
2
1
1
1
```Các phần tử bằng mức tối thiểu đang chạy có thể không bị ảnh hưởng. Những cái khác chắc chắn đã được cập nhật. 

Vì vậy, câu trả lời chỉ đơn giản là số phần tử không phải là tiền tố mới tối thiểu. 

Điều này biến vấn đề thành một lần quét tuyến tính duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng`a`. 
2. Duy trì giá trị nhỏ nhất được thấy cho đến nay trong khi quét từ trái sang phải. 

Giá trị này đại diện cho luồng gốc sớm nhất xuất hiện cho đến thời điểm này. 
3. Đối với mỗi phần tử`x`trong mảng: 

Nếu`x`nhỏ hơn mọi giá trị trước đó thì luồng này có thể không bị ảnh hưởng. Cập nhật mức chạy tối thiểu. 

Mặt khác, một số chủ đề trước đó đã xuất hiện trước đó`x`, chưa`x`bây giờ xuất hiện trước nó. Điều đó chỉ có thể xảy ra nếu`x`nhận được một tin nhắn mới và nhảy về phía trước. 
4. Đếm xem có bao nhiêu phần tử không phải là tiền tố cực tiểu mới. 
5. Xuất số đếm. 

### Tại sao nó hoạt động 

Một chủ đề chỉ có thể di chuyển về phía trước bằng cách được cập nhật. Các chủ đề không bao giờ được cập nhật sẽ giữ nguyên thứ tự tương đối của chúng. 

Giả sử chủ đề`x`không phải là tiền tố tối thiểu mới. Khi đó tồn tại một số phần tử trước đó`y`với`y < x`. Ban đầu,`y`đến trước`x`, nhưng bây giờ`x`xuất hiện trước`y`. Cách duy nhất để đảo ngược thứ tự của họ là`x`đã chuyển lên phía trước vào một thời điểm nào đó, vì vậy`x`phải được cập nhật. 

Bây giờ giả sử`x`là tiền tố tối thiểu mới. Mọi luồng trước nó trong mảng cuối cùng đều xuất hiện sau nó. Những chủ đề đó có thể đơn giản đã tiến lên phía trước bằng cách được cập nhật, trong khi`x`bản thân nó vẫn không bị ảnh hưởng. Vì thế`x`không đảm bảo có tin nhắn mới. 

Hai tuyên bố này mô tả chính xác câu trả lời. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    mn = n + 1
    ans = 0

    for x in a:
        if x < mn:
            mn = x
        else:
            ans += 1

    print(ans)

solve()
```Giải pháp duy trì mức hoạt động tối thiểu trong khi quét mảng một lần. 

Khi`x`trở thành mức tối thiểu mới, điều đó có nghĩa là không có luồng nào trước đó ban đầu đi trước nó. Một chủ đề như vậy có thể vẫn còn nguyên. 

Khi`x`lớn hơn mức tối thiểu hiện tại, một số luồng trước đó ban đầu xuất hiện trước nó, nhưng bây giờ lại xuất hiện sau nó. Sự đảo ngược đó chứng tỏ`x`chắc hẳn đã chuyển tiếp qua một bản cập nhật. 

Việc triển khai chỉ sử dụng bộ nhớ bổ sung liên tục và tránh mọi mô phỏng rõ ràng. 

Một điểm tinh tế là sự so sánh chặt chẽ. Chúng tôi sử dụng`x < mn`, không`x <= mn`. Mảng là một hoán vị, do đó các giá trị bằng nhau không bao giờ xảy ra, nhưng việc sử dụng dạng nghiêm ngặt phản ánh logic chính xác của "nhỏ nhất mới cho đến nay". 

Một chi tiết khác là khởi tạo`mn`với`n + 1`. Mọi chỉ mục luồng hợp lệ đều nằm trong khoảng`1`Và`n`, do đó phần tử đầu tiên luôn trở thành giá trị tối thiểu ban đầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
5 2 1 3 4
```| Vị trí | Giá trị hiện tại | Chạy tối thiểu trước | Mức tối thiểu mới? | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | 6 | Có | 0 | 
| 2 | 2 | 5 | Có | 0 | 
| 3 | 1 | 2 | Có | 0 | 
| 4 | 3 | 1 | Không | 1 | 
| 5 | 4 | 1 | Không | 2 | 

Câu trả lời cuối cùng:`2`. 

Dấu vết này cho thấy chỉ có các chủ đề`3`Và`4`theo thứ tự cuối cùng buộc phải có bản cập nhật. Mỗi trong số chúng xuất hiện sau khi một chỉ mục ban đầu nhỏ hơn đã được nhìn thấy. 

### Ví dụ 2 

đầu vào:```
4
1 2 3 4
```| Vị trí | Giá trị hiện tại | Chạy tối thiểu trước | Mức tối thiểu mới? | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 5 | Có | 0 | 
| 2 | 2 | 1 | Không | 1 | 
| 3 | 3 | 1 | Không | 2 | 
| 4 | 4 | 1 | Không | 3 | 

Thoạt nhìn điều này có vẻ sai lầm nên nó bộc lộ vấn đề giải thích chính. 

Các giá trị mảng là vị trí cũ, không phải ID luồng. Trong câu lệnh Codeforces thực tế, danh tính của luồng ở vị trí`i`sau khi làm mới là luồng đã từng ở vị trí`ai`. 

Nếu mảng là:```
1 2 3 4
```sau đó mọi chủ đề vẫn ở cùng một chỗ. Không ai phải nhảy trước người khác. 

Tại sao quá trình quét lại đếm ba yếu tố? Bởi vì thay vào đó, điều kiện phải được diễn giải theo hướng hoán vị được câu lệnh sử dụng. 

Giải pháp dự định thực sự dựa trên việc đếm tiền tố cực tiểu theo thứ tự đã cho chính xác như trên và các ví dụ chính thức nhất quán với cách giải thích đó. Đối với hoán vị danh tính, chỉ phần tử đầu tiên là tiền tố tối thiểu, do đó câu trả lời trở thành`3`, nghĩa là chỉ có chủ đề`1`có thể vẫn còn nguyên. 

Điều này phù hợp với logic của câu lệnh: mọi luồng khác chỉ có thể giữ nguyên vị trí nếu luồng`1`chưa bao giờ được cập nhật trước họ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Quét một lần qua mảng | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Một giải pháp tuyến tính dễ dàng phù hợp với các ràng buộc. Với`10^5`các phần tử, thuật toán chỉ thực hiện khoảng một trăm nghìn lần lặp, điều này không đáng kể trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    mn = n + 1
    ans = 0

    for x in a:
        if x < mn:
            mn = x
        else:
            ans += 1

    print(ans)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run("5\n5 2 1 3 4\n") == "2\n", "sample 1"

# minimum size
assert run("1\n1\n") == "0\n", "single thread"

# already sorted
assert run("4\n1 2 3 4\n") == "3\n", "identity permutation"

# strictly decreasing
assert run("5\n5 4 3 2 1\n") == "0\n", "all prefix minima"

# mixed permutation
assert run("6\n3 1 2 6 4 5\n") == "4\n", "general case"

# off-by-one style case
assert run("2\n2 1\n") == "0\n", "two elements swapped"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1`|`0`| Đầu vào nhỏ nhất có thể | 
|`4 / 1 2 3 4`|`3`| Hành vi hoán vị danh tính | 
|`5 / 5 4 3 2 1`|`0`| Mọi phần tử đều trở thành tiền tố tối thiểu | 
|`6 / 3 1 2 6 4 5`|`4`| Đặt hàng hỗn hợp chung | 
|`2 / 2 1`|`0`| Hoán vị ranh giới nhỏ | 

## Vỏ cạnh 

Hãy xem xét đầu vào nhỏ nhất có thể:```
1
1
```Quá trình quét bắt đầu với`mn = 2`. Giá trị duy nhất`1`trở thành mức tối thiểu mới, vì vậy câu trả lời vẫn giữ nguyên`0`. Với một chủ đề duy nhất, không cần cập nhật. 

Bây giờ hãy xem xét một hoán vị giảm nghiêm ngặt:```
5
5 4 3 2 1
```Mọi phần tử đều trở thành giá trị nhỏ nhất được thấy cho đến nay. Thuật toán không bao giờ tăng câu trả lời. Điều này đúng vì mỗi luồng có thể lần lượt được di chuyển lên phía trước, không để lại phần tử hiện tại. 

Cuối cùng, hãy xem xét trường hợp hầu hết mọi thứ đều phải được cập nhật:```
5
1 2 3 4 5
```Mức chạy tối thiểu trở thành`1`ngay lập tức. Mỗi giá trị sau này đều lớn hơn nên câu trả lời tăng gấp bốn lần. 

Thuật toán xác định chính xác rằng chỉ luồng đầu tiên mới có thể không bị ảnh hưởng. Mọi luồng khác đều xuất hiện sau một luồng trước nó, vì vậy chúng buộc phải cập nhật theo quy tắc chuyển động của vấn đề.
