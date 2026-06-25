---
title: "CF 105229E - \u65e0\u7ebf\u8f6f\u4ef6\u65e5"
description: "Chúng ta được cung cấp một chuỗi dài các chữ cái và chúng ta được phép tự do sắp xếp lại bất kỳ chữ cái nào sau khi “cắt” chúng ra khỏi văn bản gốc. Từ nhiều tập hợp chữ cái này, chúng tôi muốn tạo thành từ "Thượng Hải" nhiều lần nhất có thể, trong đó trường hợp chữ cái không quan trọng."
date: "2026-06-24T16:08:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105229
codeforces_index: "E"
codeforces_contest_name: "The 2024 Shanghai Collegiate Programming Contest"
rating: 0
weight: 105229
solve_time_s: 48
verified: true
draft: false
---

[CF 105229E - \u65e0\u7ebf\u8f6f\u4ef6\u65e5](https://codeforces.com/problemset/problem/105229/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi dài các chữ cái và chúng ta được phép tự do sắp xếp lại bất kỳ chữ cái nào sau khi “cắt” chúng ra khỏi văn bản gốc. Từ nhiều tập hợp chữ cái này, chúng tôi muốn tạo thành từ "Thượng Hải" nhiều lần nhất có thể, trong đó trường hợp chữ cái không quan trọng. Mỗi khi chúng tôi tập hợp thành công một bản sao đầy đủ của từ đó, chúng tôi sẽ loại bỏ các chữ cái đó và tính một giải thưởng. 

Nhiệm vụ là xác định xem có thể tạo được bao nhiêu bản sao rời rạc của từ “Thượng Hải” từ số lượng chữ cái có sẵn. Vì việc sắp xếp lại không bị hạn chế nên thứ tự trong chuỗi gốc không liên quan. Chỉ có tần số của mỗi chữ cái là quan trọng. 

Kích thước đầu vào có thể lên tới 1e6 ký tự, điều này ngay lập tức ngụ ý rằng mọi giải pháp đều phải tuyến tính theo độ dài của chuỗi. Bất cứ điều gì liên quan đến việc quét lặp đi lặp lại hoặc mô phỏng theo từng đội hình sẽ trở nên quá chậm. Một tần số đếm lượt duy nhất là khả thi, nhưng mô phỏng lồng nhau hoặc loại bỏ tham lam trên mỗi từ sẽ không khả thi. 

Một điểm tinh tế là không phân biệt chữ hoa chữ thường. Điều này có nghĩa là 'S' và 's' giống hệt nhau và tương tự đối với tất cả các chữ cái trong "Thượng Hải". Bất kỳ việc triển khai nào quên chuẩn hóa sẽ tính hai lần hoặc bỏ lỡ các chữ cái hợp lệ. 

Không có trường hợp cạnh cấu trúc phức tạp nào ngoài sự mất cân bằng tần số. Ví dụ: nếu chuỗi không chứa 'a', thì việc triển khai đơn giản giả định ít nhất một lần xuất hiện trên mỗi từ có thể chia cho 0 hoặc tiến hành không chính xác. Một trường hợp góc khác là khi một chữ cái chiếm ưu thế nhưng các chữ cái khác bị thiếu hoàn toàn, dẫn đến không có từ hợp lệ nào mặc dù kích thước đầu vào lớn. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ cố gắng mô phỏng từng từ xây dựng. Chúng tôi có thể quét liên tục nhiều tập hợp có sẵn, loại bỏ một lần xuất hiện của mỗi chữ cái được yêu cầu và tăng bộ đếm cho đến khi chúng tôi không còn có thể tạo thành từ đó nữa. Điều này đúng vì nó phản ánh trực tiếp quá trình xây dựng từ ngữ. 

Tuy nhiên, điều này là không hiệu quả. Trong trường hợp xấu nhất, giả sử chúng ta có thể tạo ra k bản sao của “Thượng Hải”. Mỗi lần thử quét tối đa O(n) ký tự để xác minh tính khả dụng và chúng tôi lặp lại điều này k lần. Điều này dẫn đến O(k·n), suy biến thành O(n²) khi k tỉ lệ với n. Với n lên tới 1e6, điều này rõ ràng là không khả thi. 

Quan sát quan trọng là mỗi cách hình thành từ đều độc lập về mặt số lượng. Chúng tôi không chọn chữ cái nào để sử dụng một cách linh hoạt; chúng ta chỉ cần biết tổng cộng mỗi chữ cái được yêu cầu xuất hiện bao nhiêu lần. Vì mỗi từ “Thượng Hải” yêu cầu số lượng ký tự cố định nên câu trả lời được xác định bởi tần suất ký tự thắt cổ chai. 

Cụ thể, “Thượng Hải” gồm các chữ cái: S, h, a, n, g, h, a, i. Sau khi chuẩn hóa, chúng ta cần đếm: 

S:1, h:2, a:2, n:1, g:1, i:1. 

Vì vậy, số lượng từ đầy đủ mà chúng ta có thể tạo thành là tối thiểu trên tất cả các chữ cái bắt buộc của: 

tần số(chữ cái) chia cho require_count(chữ cái). 

Điều này làm giảm vấn đề về số đếm tần số đơn giản và một vài phép chia số nguyên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force | O(n²) | O(n) | Quá chậm | 
| Đếm tần số | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi mọi ký tự trong chuỗi đầu vào thành chữ thường. Điều này đảm bảo việc phân biệt chữ hoa chữ thường được xử lý thống nhất. Nếu không có điều này, các nhóm tần số riêng biệt cho chữ hoa và chữ thường sẽ phân chia không chính xác các chữ cái có thể sử dụng được. 
2. Đếm tần số của mỗi ký tự bằng cách sử dụng một mảng có kích thước 26. Vì chúng ta chỉ xử lý các chữ cái tiếng Anh nên điều này là đủ và tránh được chi phí từ điển. 
3. Xác định mẫu tần số cần thiết cho từ “shanghai”:

s → 1, h → 2, a → 2, n → 1, g → 1, i → 1. 
4. Đối với mỗi chữ cái được yêu cầu này, hãy tính xem nó có thể được đáp ứng bao nhiêu lần bằng cách chia tần số khả dụng cho số lượng được yêu cầu. Ví dụ: nếu chúng ta có 5 'h' thì nó đóng góp 2 bản đầy đủ. 
5. Câu trả lời là giá trị nhỏ nhất của tất cả các thương số này. Mức tối thiểu này thể hiện nguồn lực giới hạn trong số tất cả các chữ cái được yêu cầu. 

Lý do đằng sau việc lấy mức tối thiểu là mỗi từ tiêu thụ một đơn vị của mỗi bó chữ cái bắt buộc. Ngay cả khi tất cả các chữ cái ngoại trừ một chữ cái đều phong phú, thì chữ cái giới hạn sẽ giới hạn số lượng công trình hoàn chỉnh. 

### Tại sao nó hoạt động 

Mỗi “Thượng Hải” hợp lệ sẽ sử dụng một tập hợp nhiều chữ cái cố định. Bất kỳ giải pháp nào cũng tương ứng với việc chọn k bản sao của multiset này từ nhóm tần số chung. Đối với k cố định, tính khả thi yêu cầu mỗi chữ cái trong đầu vào ít nhất phải gấp k lần tần số yêu cầu của nó. Điều này vừa cần vừa đủ vì các chữ cái là nguồn tài nguyên độc lập một khi lệnh bị bỏ qua. Do đó, k tối đa chính xác là tỷ lệ tối thiểu trên tất cả các chữ cái được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    s = input().strip().lower()

    freq = [0] * 26
    for ch in s:
        freq[ord(ch) - 97] += 1

    need = {
        's': 1,
        'h': 2,
        'a': 2,
        'n': 1,
        'g': 1,
        'i': 1
    }

    ans = float('inf')
    for c, req in need.items():
        ans = min(ans, freq[ord(c) - 97] // req)

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách chuẩn hóa chuỗi, đảm bảo rằng chữ hoa và chữ thường nằm trong cùng một không gian tần số. Sau đó, nó xây dựng một mảng tần số cố định, cho phép truy cập O(1) vào bất kỳ số lượng ký tự nào. 

Từ điển`need`mã hóa cấu trúc của từ đích. Đối với mỗi chữ cái được yêu cầu, chúng tôi tính toán số lượng bản sao đầy đủ mà nguồn cung cấp hiện tại hỗ trợ. Mức tối thiểu cuối cùng là yếu tố giới hạn. 

Một lỗi triển khai phổ biến là quên 'h' và 'a' xuất hiện hai lần trong từ. Coi chúng như những lần xuất hiện đơn lẻ sẽ đánh giá quá cao câu trả lời. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
ShangHaiShiSaiHaiGeTongKuai
```Chúng tôi bình thường hóa và đếm tần số. Các chữ cái chính: 

| Bước | s | h | một | n | g | tôi | tối thiểu hiện tại | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| bắt đầu | 0 | 0 | 0 | 0 | 0 | 0 | thông tin | 
| sau khi đếm | 4 | 4 | 4 | 2 | 2 | 4 | - | 

Bây giờ tính tỉ số: 

| Thư | tần số | cần | tỷ lệ | 
| --- | --- | --- | --- | 
| s | 4 | 1 | 4 | 
| h | 4 | 2 | 2 | 
| một | 4 | 2 | 2 | 
| n | 2 | 1 | 2 | 
| g | 2 | 1 | 2 | 
| tôi | 4 | 1 | 4 | 

Tối thiểu là 2, vì vậy chúng ta có thể tạo thành 2 từ. 

Điều này cho thấy rằng mặc dù có nhiều chữ cái nhưng 'h' và 'a' vẫn hạn chế việc xây dựng. 

### Ví dụ 2 

đầu vào:```
shg
```Tần số: 

s=1, h=1, g=1, khác=0 

| Thư | tỷ lệ | 
| --- | --- | 
| s | 1 | 
| h | 0 | 
| một | 0 | 
| n | 0 | 
| g | 1 | 
| tôi | 0 | 

Tối thiểu là 0, xác nhận rằng nếu không có tất cả các chữ cái bắt buộc thì không thể tạo thành từ. 

Điều này xác nhận thuật toán xử lý chính xác các trường hợp thiếu chữ cái. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần đếm tần số, sau đó đếm liên tục trên 6 chữ cái | 
| Không gian | O(1) | Mảng cố định có kích thước 26 | 

Giải pháp này phù hợp một cách thoải mái trong các giới hạn từ n đến 1e6, vì sau đó nó chỉ thực hiện quét tuyến tính và số học theo thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import sys as _sys
    out = io.StringIO()
    _stdout = _sys.stdout
    _sys.stdout = out
    solve()
    _sys.stdout = _stdout
    return out.getvalue().strip()

# provided sample
assert run("""27
ShangHaiShiSaiHaiGeTongKuai
""") == "2"

# minimum size, cannot form anything
assert run("""3
shg
""") == "0"

# exactly one word
assert run("""8
ShangHai
""") == "1"

# multiple copies
assert run("""16
ShangHaiShangHai
""") == "2"

# missing a key letter
assert run("""10
ssssssssss
""") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| shg | 0 | thiếu chữ cái bắt buộc | 
| Thượng Hải | 1 | hình thành chính xác | 
| Thượng HảiShangHai | 2 | nhiều bản sao độc lập | 
| tất cả 's' | 0 | xử lý tắc nghẽn không trường hợp | 

## Vỏ cạnh 

Trường hợp quan trọng là khi một hoặc nhiều chữ cái bắt buộc không bao giờ xuất hiện. Ví dụ: 

đầu vào:```
5
aaaaa
```Ở đây chỉ có 'a' hiện diện. Tần số của các chữ cái bắt buộc khác bằng 0 nên tỷ lệ của chúng bằng 0. Thuật toán tính tỷ lệ tối thiểu = 0, trả về chính xác 0. Một mô phỏng đơn giản có thể cố gắng phân chia hoặc giả định ít nhất một phần xây dựng và thất bại về mặt logic. 

Một trường hợp khác là phân bố không đều: 

đầu vào:```
12
shanghaishh
```Số lượng có thể cung cấp đủ hầu hết các chữ cái nhưng không đủ yêu cầu 'a' hoặc 'h' trùng lặp. Thuật toán vẫn chia chính xác cho bội số cần thiết và lấy mức tối thiểu, đảm bảo rằng các chữ cái được biểu thị quá mức không thể làm tăng kết quả. 

Những trường hợp này xác nhận rằng việc coi vấn đề như một nút thắt cổ chai đối với các nguồn lực độc lập là cần thiết và đủ.
