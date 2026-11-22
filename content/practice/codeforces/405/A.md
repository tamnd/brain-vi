---
title: "CF 405A - Lật Trọng Lực"
description: "Chúng ta được cấp một dãy các khối lập phương thẳng đứng. Mỗi vị trí trong hàng chứa một cột và mảng đầu vào mô tả có bao nhiêu khối được xếp chồng lên nhau ở mỗi vị trí. Sau đó, một “công tắc trọng lực” xảy ra. Trước khi chuyển đổi, trọng lực tác động hướng xuống nên mỗi cột đều ổn định và độc lập."
date: "2026-06-07T01:37:13+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 405
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 238 (Div. 2)"
rating: 900
weight: 405
solve_time_s: 256
verified: true
draft: false
---

[CF 405A - Lật trọng lực](https://codeforces.com/problemset/problem/405/A) 

**Xếp hạng:** 900 
**Tags:** tham lam, thực hiện, sắp xếp 
**Thời gian giải:** 4 phút 16s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một dãy các khối lập phương thẳng đứng. Mỗi vị trí trong hàng chứa một cột và mảng đầu vào mô tả có bao nhiêu khối được xếp chồng lên nhau ở mỗi vị trí. 

Sau đó, một “công tắc trọng lực” xảy ra. Trước khi chuyển đổi, trọng lực tác động hướng xuống nên mỗi cột đều ổn định và độc lập. Sau khi chuyển đổi, trọng lực bắt đầu tác động sang bên phải, khiến các hình khối trượt theo chiều ngang. Khối lập phương luôn di chuyển sang phải càng xa càng tốt, lấp đầy các vị trí sẵn có thấp hơn trong các cột bên phải của nó trước khi giữ nguyên vị trí cũ. 

Sau khi mọi thứ ổn định, chúng ta cần báo cáo có bao nhiêu hình khối ở mỗi cột. 

Sự thay đổi tinh thần quan trọng là không có gì được tạo ra hoặc bị phá hủy, chỉ được sắp xếp lại dưới một lực định hướng. Trạng thái cuối cùng chỉ phụ thuộc vào cách các khối phân phối lại khi rơi liên tục về phía bên phải. 

Các ràng buộc rất nhỏ: tối đa 100 cột và tối đa 100 khối trên mỗi cột. Ngay cả một mô phỏng chậm, chạm vào từng khối nhiều lần vẫn sẽ chạy thoải mái trong giới hạn, vì tổng số khối nhiều nhất là 10.000 và bất kỳ hành vi bậc hai nào trên 100 phần tử đều không đáng kể. 

Một số tình huống có thể khiến bạn phải suy nghĩ ngây thơ. 

Ví dụ: nếu tất cả các cột đã có cùng chiều cao`3 3 3`, thì không có gì thay đổi sau trọng lực, bởi vì không có cấu hình nào “thấp hơn” có thể đạt được bằng cách sắp xếp lại. 

Nếu mảng đang giảm như`5 4 3 2`, trạng thái cuối cùng sẽ được sắp xếp thành`2 3 4 5`. Một sai lầm phổ biến là giả sử trọng lực giữ nguyên thứ tự ban đầu của các cột trong khi chỉ dịch chuyển các hình khối, nhưng trên thực tế, sự phân bố lại hoàn toàn sắp xếp lại chiều cao. 

Nếu chỉ có một cột, chẳng hạn như`n = 1`, câu trả lời giống hệt với dữ liệu đầu vào, vì không có nơi nào để các hình khối di chuyển. 

## Phương pháp tiếp cận 

Một cách trực tiếp để hình dung về quá trình này là mô phỏng hoạt động của mỗi khối. Bạn có thể tưởng tượng việc lặp đi lặp lại trên mảng và để các hình khối “rơi đúng” từng bước bất cứ khi nào cột tiếp theo có khoảng trống so với cấu hình hiện tại. Tuy nhiên, quan điểm này nhanh chóng trở nên không hiệu quả vì khối lập phương có thể cần đi qua nhiều cột và sự tương tác giữa các cột có nghĩa là bạn sẽ phải kiểm tra lại nhiều lần tính ổn định. 

Nếu chúng ta cố gắng hình thức hóa mô phỏng đơn giản này, thì mỗi khối có thể được di chuyển lên đến`n`các vị trí và có tới`n * 100`tổng số hình khối. Điều này dẫn đến hành vi trong trường hợp xấu nhất xung quanh`O(n^2 * max height)`điều này là không cần thiết đối với một vấn đề cấu trúc nhỏ như vậy. 

Quan sát quan trọng là trọng lực hoàn toàn không phụ thuộc vào vị trí mà chỉ phụ thuộc vào nhiều chiều cao cột. Khi trọng lực tác động sang bên phải, các hình khối sẽ “tự sắp xếp” một cách hiệu quả thành các cột có chiều cao không giảm từ trái sang phải. Mọi cấu hình đều hội tụ về cùng một trạng thái: phiên bản được sắp xếp của mảng ban đầu. 

Điều này xảy ra vì bất kỳ sự đảo ngược nào, trong đó cột bên trái cao hơn cột bên phải, có thể được giải quyết bằng cách dịch chuyển các khối sang bên phải cho đến khi chiều cao được sắp xếp theo thứ tự. Việc áp dụng nhiều lần ý tưởng này sẽ loại bỏ tất cả các sự đảo ngược, đó chính xác là công việc của việc sắp xếp. 

Vì vậy, toàn bộ vấn đề giảm xuống việc sắp xếp mảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n² · tối đa(a)) | O(1) | Quá chậm và không cần thiết | 
| Sắp xếp | O(n log n) | O(1) hoặc O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số cột và mảng độ cao. Tại thời điểm này, chúng tôi chỉ hiểu mảng là một tập hợp các giá trị chứ không phải vị trí vì vị trí sẽ không quan trọng sau khi trọng lực tác động. 
2. Sắp xếp mảng theo thứ tự không giảm. Bước này trực tiếp xây dựng cấu hình ổn định cuối cùng dưới tác dụng của trọng lực hướng về bên phải vì tất cả các hình khối phải ở càng về bên phải càng tốt, điều này buộc các ngăn xếp nhỏ hơn phải xuất hiện trước tiên. 
3. Xuất mảng đã sắp xếp dưới dạng chiều cao cột cuối cùng. 

### Tại sao nó hoạt động 

Quá trình này bảo toàn tổng số khối và chỉ cho phép phân phối lại về bên phải. Bất kỳ cấu hình nào không được sắp xếp đều chứa ít nhất một đảo ngược liền kề trong đó cột lớn hơn đứng trước cột nhỏ hơn. Trọng lực loại bỏ sự đảo ngược như vậy bằng cách di chuyển các hình khối sang phải một cách hiệu quả. Vì các phép nghịch đảo luôn có thể được giải quyết và không có thao tác nào tạo ra các phép nghịch đảo mới theo hướng ngược lại, nên hệ thống hội tụ về một chuỗi hoàn toàn không giảm, duy nhất và bằng với mảng đã được sắp xếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input().strip())
a = list(map(int, input().split()))

a.sort()

print(*a)
```Giải pháp đọc mảng, sắp xếp và in trực tiếp. Bước sắp xếp là toàn bộ logic chuyển đổi và không cần mô phỏng. 

Điểm tinh tế duy nhất là đảm bảo định dạng đầu ra chính xác. sử dụng`print(*a)`tránh các lỗi nối thủ công và đảm bảo đầu ra được phân tách bằng dấu cách. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`4`

`3 2 1 2`Sau khi sắp xếp, chúng tôi theo dõi sự chuyển đổi. 

| Bước | Trạng thái mảng | 
| --- | --- | 
| Ban đầu | 3 2 1 2 | 
| Sau khi sắp xếp | 1 2 2 3 | 

Mảng được sắp xếp khớp với cấu hình ổn định cuối cùng dưới tác dụng của trọng lực hướng về bên phải. Ngăn xếp nhỏ hơn tích lũy ở bên trái. 

### Ví dụ 2 

đầu vào:`5`

`1 1 1 1 1`| Bước | Trạng thái mảng | 
| --- | --- | 
| Ban đầu | 1 1 1 1 1 | 
| Sau khi sắp xếp | 1 1 1 1 1 | 

Điều này cho thấy cấu hình hoàn toàn thống nhất không thay đổi vì không có sự đảo ngược nào cần giải quyết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế trong tính toán | 
| Không gian | O(1) thêm (hoặc O(n) tùy theo loại) | Cách sử dụng phụ trợ Timsort tại chỗ hoặc Python | 

Với`n ≤ 100`, việc sắp xếp diễn ra ngay lập tức, vượt xa mọi lo ngại về giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import *
    n = int(sys.stdin.readline().strip())
    a = list(map(int, sys.stdin.readline().split()))
    a.sort()
    return " ".join(map(str, a))

# provided samples
assert run("4\n3 2 1 2\n") == "1 2 2 3", "sample 1"
assert run("3\n1 2 3\n") == "1 2 3", "sample 2"

# custom cases
assert run("1\n5\n") == "5", "single column"
assert run("5\n5 4 3 2 1\n") == "1 2 3 4 5", "reverse sorted"
assert run("4\n2 2 2 2\n") == "2 2 2 2", "all equal"
assert run("6\n1 3 2 3 1 2\n") == "1 1 2 2 3 3", "mixed duplicates"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | cùng giá trị | trường hợp cạnh tối thiểu | 
| thứ tự ngược lại | sắp xếp tăng dần | trường hợp đảo ngược tồi tệ nhất | 
| tất cả đều bình đẳng | không thay đổi | ổn định khi không có chuyển động | 
| trùng lặp hỗn hợp | nhóm được sắp xếp | xử lý các giá trị lặp lại | 

## Vỏ cạnh 

cho`n = 1`, nhập như`7`tạo ra đầu ra`7`. Thuật toán đọc mảng, sắp xếp một phần tử và đưa ra kết quả không thay đổi vì việc sắp xếp không sửa đổi danh sách đơn lẻ. 

Đối với một chuỗi giảm dần như`5 4 3 2 1`, sắp xếp tạo ra`1 2 3 4 5`. Thuật toán không mô phỏng chuyển động, nó trực tiếp tính toán cấu hình cân bằng cuối cùng, tương ứng với việc loại bỏ hoàn toàn mọi nghịch đảo. 

Đối với một chuỗi thống nhất như`4 4 4 4`, việc sắp xếp khiến mảng không thay đổi. Không có sự đảo ngược nào tồn tại, do đó, tính bất biến của mảng không giảm đã được giữ ngay từ đầu và thuật toán không thực hiện chuyển đổi hiệu quả nào ngoài nhận dạng.
