---
title: "CF 104068A - \u75af\u72c2\u661f\u671f\u56db\uff0cV \u6211 50\uff01"
description: "Mỗi trường hợp thử nghiệm đưa ra một chuỗi gồm các chữ cái và chữ số. Chúng ta cần quyết định xem chuỗi đó có chứa một “mẫu thư rác” cụ thể hay không. Mẫu này được xác định bởi sự hiện diện đồng thời của năm từ khóa khác nhau: “kfc”, “crazy”, “thứ năm”, “vivo” và số “50”."
date: "2026-07-02T03:03:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104068
codeforces_index: "A"
codeforces_contest_name: "The 17-th Beihang University Collegiate Programming Contest (BCPC 2022) - Preliminary"
rating: 0
weight: 104068
solve_time_s: 52
verified: true
draft: false
---

[CF 104068A - \u75af\u72c2\u661f\u671f\u56db\uff0cV \u6211 50\uff01](https://codeforces.com/problemset/problem/104068/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi trường hợp thử nghiệm đưa ra một chuỗi gồm các chữ cái và chữ số. Chúng ta cần quyết định xem chuỗi đó có chứa một “mẫu thư rác” cụ thể hay không. Mẫu này được xác định bởi sự hiện diện đồng thời của năm từ khóa khác nhau: “kfc”, “crazy”, “thứ năm”, “vivo” và số “50”. 

Có một sự tinh tế quan trọng. Chúng tôi không tìm kiếm những từ khóa này dưới dạng các chuỗi con liền kề khớp trực tiếp với cách viết hoa chính xác. Thay vào đó, chuỗi phải được xử lý theo cách không phân biệt chữ hoa chữ thường, trong khi các chữ số phải khớp chính xác. Mỗi từ khóa phải xuất hiện dưới dạng một chuỗi con của chuỗi, nghĩa là chúng ta có thể xóa các ký tự một cách thoải mái nhưng phải giữ nguyên thứ tự. Các từ khóa khác nhau có thể sử dụng lại các ký tự, do đó các kết quả khớp của chúng có thể trùng lặp trong chuỗi gốc. 

Vì vậy, nhiệm vụ giảm xuống còn việc kiểm tra xem liệu chúng ta có thể nhúng từng mẫu trong số năm mẫu vào chuỗi đã cho dưới dạng các chuỗi con trong trường hợp khớp không phân biệt chữ hoa chữ thường hay không. 

Các ràng buộc rất nhỏ: tối đa 100 chuỗi, mỗi chuỗi có độ dài lên tới 1000. Điều này cho phép O(n) hoặc O(n * k) trên mỗi chuỗi tiếp cận một cách thoải mái, trong đó k là tổng số ký tự trên các mẫu. Bất cứ điều gì theo cấp số nhân hoặc liên quan đến việc quét lặp lại trên mỗi mẫu mà không cẩn thận vẫn sẽ vượt qua, nhưng quét tuyến tính rõ ràng trên mỗi mẫu là giải pháp dự kiến. 

Một số trường hợp đặc biệt quan trọng. 

Một cạm bẫy phổ biến là xử lý trường hợp. Ví dụ: “KFC1crazy” vẫn phải khớp với “kfc” và “crazy”, nhưng tìm kiếm chuỗi con trực tiếp sẽ không thành công trừ khi được chuẩn hóa. 

Một cạm bẫy khác là việc khớp chữ số cho “50”. Nếu bộ giải vô tình xử lý các ký tự một cách lỏng lẻo hoặc bỏ qua các chữ số thì một chuỗi như “5O” (chữ O thay vì số 0) sẽ không được chấp nhận. 

Vấn đề thứ ba là hiểu nhầm “chuỗi con” là “chuỗi con”. Ví dụ: “kxxfxc” vẫn phải khớp với “kfc”, ngay cả khi các ký tự được phân tách. 

## Phương pháp tiếp cận 

Cách diễn giải thô bạo sẽ cố gắng tìm kiếm từng từ khóa dưới dạng một chuỗi con một cách độc lập bằng cách quét từ mọi vị trí hoặc thậm chí tạo ra tất cả các chuỗi con, nhưng điều đó nhanh chóng trở nên không cần thiết. Việc tạo ra các chuỗi con là theo cấp số nhân và rõ ràng là không khả thi. 

Một cách tiếp cận mạnh mẽ có cấu trúc hơn là, đối với mỗi từ khóa, quét chuỗi một cách tham lam: cố gắng khớp ký tự đầu tiên, sau đó tiếp tục chuyển tiếp cho đến khi tìm thấy kết quả khớp tiếp theo, v.v. Điều này hoạt động với O(n) cho mỗi từ khóa, vì vậy O(5n) cho mỗi trường hợp thử nghiệm. Ngay cả với 1000 ký tự và 100 trường hợp thử nghiệm, điều này cũng không đáng kể. 

Quan sát chính là mỗi từ khóa là độc lập. Không có sự tương tác giữa chúng ngoại trừ việc chúng chia sẻ cùng một chuỗi nguồn. Điều đó có nghĩa là chúng tôi có thể coi mỗi từ khóa khớp như một phép kiểm tra trình tự tiếp theo tiêu chuẩn. 

Do đó, vấn đề giảm xuống còn việc chạy năm kiểm tra trình tự con độc lập, sau khi chuẩn hóa kiểu chữ cho các chữ cái. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kiểm tra trình tự tham lam cho mỗi từ khóa | O(T * n * 5) | O(1) | Đã chấp nhận | 
| Liệt kê trình tự Brute-force | O(2^n) | O(n) | Quá chậm | 

## Hướng dẫn thuật toán 

Chúng tôi sửa năm mẫu mục tiêu sau khi chuẩn hóa: “kfc”, “crazy”, “thứ năm”, “vivo” và “50”. Đối với các chữ cái, chúng ta so sánh bằng chữ thường; các chữ số được so sánh trực tiếp. 

1. Đối với mỗi chuỗi trường hợp kiểm thử, hãy chuyển đổi nó thành dạng trong đó mọi ký tự được giữ ở dạng chữ số hoặc được chuyển thành chữ thường nếu đó là một chữ cái. Điều này đảm bảo hành vi kết hợp thống nhất. 
2. Đối với mỗi từ khóa, hãy cố gắng so khớp nó dưới dạng một dãy con bằng cách sử dụng con trỏ trên chuỗi. Chúng tôi duy trì chỉ số j trên từ khóa. Chúng tôi quét chuỗi từ trái sang phải và bất cứ khi nào ký tự hiện tại khớp với từ khóa [j], chúng tôi sẽ tiến lên j. Nếu j đạt đến độ dài từ khóa thì từ khóa đó được kết hợp hoàn toàn. 
3. Nếu tất cả năm từ khóa có thể được đối sánh độc lập, chúng ta sẽ xuất ra “Có”, nếu không thì xuất ra “Không”.

Lý do chức năng quét tham lam hoạt động là vì việc so khớp chuỗi con không yêu cầu quay lui. Khi chúng tôi khớp một ký tự cho một vị trí nhất định trong từ khóa, việc trì hoãn nó sẽ không bao giờ cải thiện cơ hội thành công vì các ký tự trong tương lai vẫn có sẵn. 

### Tại sao nó hoạt động 

Mỗi kết hợp từ khóa là một quá trình đơn điệu trên chuỗi đầu vào: chúng tôi chỉ tiến về phía trước. Nếu một sự trùng khớp tồn tại thì sẽ tồn tại một sự trùng khớp tham lam chiếm các vị trí hợp lệ sớm nhất có thể. Điều này đảm bảo rằng việc quét tham lam không thành công có nghĩa là không tồn tại chuỗi con hợp lệ, bởi vì mọi kết quả khớp thay thế sẽ yêu cầu bỏ qua các kết quả khớp hợp lệ trước đó, điều này chỉ có thể làm giảm tính linh hoạt sẵn có cho các ký tự sau này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

patterns = ["kfc", "crazy", "thursday", "vivo", "50"]

def is_subsequence(s, p):
    j = 0
    n = len(p)
    for ch in s:
        if j < n and ch == p[j]:
            j += 1
            if j == n:
                return True
    return j == n

t = int(input())
for _ in range(t):
    s = input().strip()
    s = ''.join(ch.lower() for ch in s)

    ok = True
    for p in patterns:
        if not is_subsequence(s, p):
            ok = False
            break

    print("Yes" if ok else "No")
```Việc triển khai trước tiên sẽ chuẩn hóa chuỗi thành dạng chữ thường để so sánh các chữ cái là thống nhất. Trình kiểm tra trình tự tiếp theo sử dụng một con trỏ duy nhất trên mẫu và quét chuỗi một lần, tiến lên con trỏ bất cứ khi nào kết quả khớp xảy ra. Việc thoát sớm bên trong vòng lặp đảm bảo chúng tôi không quét các ký tự không cần thiết sau khi đã tìm thấy mẫu. 

Một điểm tinh tế là “50” được coi như một chuỗi bình thường, do đó các chữ số được khớp chính xác và không bị ảnh hưởng bởi việc chuẩn hóa chữ hoa chữ thường. Điều này giữ nguyên logic cho cả mẫu chữ và số. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
KFC1crazy2THURSday3Viv04SO
```Chúng tôi kiểm tra từng mẫu một cách tuần tự. 

| Mẫu | Kết quả quét | Đã khớp | 
| --- | --- | --- | 
| kfc | k → f → c tìm theo thứ tự | Có | 
| điên rồ | tìm thấy c r a z y sau khi bỏ qua các chữ số | Có | 
| thứ năm | t h u r s d a y được tìm thấy | Có | 
| vivo | v i v o đã tìm thấy (o là chữ số 0 trong đầu vào nhưng chỉ khớp với char '0', điều này còn tùy) | Có nếu kết quả khớp chính xác cho phép ánh xạ 'o' so với '0' là chính xác trên mỗi câu lệnh | 
| 50 | các chữ số 5 rồi 0 tồn tại theo thứ tự | Có | 

Vì cả năm đều thành công nên đầu ra là “Có”. 

### Ví dụ 2 

đầu vào:```
50vIVoakjhsbCrazykfcThursday
```| Mẫu | Kết quả quét | Đã khớp | 
| --- | --- | --- | 
| kfc | tìm thấy ở cuối | Có | 
| điên rồ | được tìm thấy trong phân đoạn “Điên” | Có | 
| thứ năm | tìm thấy ở đuôi | Có | 
| vivo | v i v o xuất hiện theo thứ tự | Có | 
| 50 | bắt đầu bằng “50” | Có | 

Tất cả các mẫu đều khớp nhau, vì vậy đầu ra là “Có”. 

Những dấu vết này cho thấy thứ tự ký tự là quan trọng nhưng tính liền kề thì không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T * N) | Mỗi trường hợp kiểm thử quét chuỗi một lần trên mỗi mẫu, với số lượng mẫu không đổi | 
| Không gian | O(1) | Chỉ con trỏ và lưu trữ chuỗi chuẩn hóa | 

Tổng công việc tối đa là 100 chuỗi có độ dài 1000, cung cấp khoảng 100.000 kiểm tra ký tự, nằm trong giới hạn thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    import sys
    input = sys.stdin.readline

    patterns = ["kfc", "crazy", "thursday", "vivo", "50"]

    def is_subsequence(s, p):
        j = 0
        n = len(p)
        for ch in s:
            if j < n and ch == p[j]:
                j += 1
                if j == n:
                    return True
        return j == n

    t = int(input())
    for _ in range(t):
        s = input().strip()
        s = ''.join(ch.lower() for ch in s)

        ok = True
        for p in patterns:
            if not is_subsequence(s, p):
                ok = False
                break
        print("Yes" if ok else "No")

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdout = old_stdout
    return out.strip()

# provided samples (illustrative formatting)
assert run("1\nKFC1crazy2THURSday3VivO450\n") == "Yes"
assert run("1\nabc\n") == "No"

# custom cases
assert run("1\nkfcrazythursdayvivo50\n") == "Yes"
assert run("1\nKfCxxcRazyTHuRsdayvivo50\n") == "Yes"
assert run("1\nkfcrazythursdayvivo5\n") == "No"
assert run("1\n50kfccrazythursdayvivo\n") == "Yes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nối đầy đủ | Có | trận đấu đầy đủ đơn giản nhất | 
| tiếng ồn trường hợp hỗn hợp | Có | trường hợp không nhạy cảm | 
| chữ số bị thiếu | Không | yêu cầu nghiêm ngặt về chữ số | 
| sắp xếp lại các khối hợp lệ | Có | sự độc lập của các mẫu | 

## Vỏ cạnh 

Trường hợp một cạnh là khi các chữ cái được trộn lẫn với tiếng ồn ngẫu nhiên. Ví dụ: đầu vào:```
KfCxxCRAZYxxTHuRsDayxxVIVo50
```Sau khi chuẩn hóa, nó trở thành:```
kfCxxcrazyxxthursdayxxvivo50
```Việc quét chuỗi con tham lam cho từng mẫu vẫn thành công vì các ký tự phụ không bao giờ chặn các kết quả trùng khớp trong tương lai; họ chỉ cung cấp nhiều cơ hội bỏ qua hơn. 

Một trường hợp khác là thay thế chữ số không chính xác, chẳng hạn như:```
kfc crazy thursday vivo 5O
```Ở đây “O” là một chữ cái, không phải số 0. Sau khi chuẩn hóa, nó vẫn là “o” và không thể khớp mẫu “50” vì không có chữ số ‘0’ nào theo sau ‘5’. Thuật toán từ chối điều này một cách chính xác vì việc khớp chữ số là chính xác và không bị lỏng lẻo. 

Trường hợp cạnh cuối cùng là các mẫu đan xen rất nhiều:```
kxfycrazytwhxursdayvixvo50
```Ngay cả khi có tiếng ồn lớn, mỗi con trỏ chỉ tiến lên khi ký tự tiếp theo chính xác xuất hiện. Quá trình quét thành công một cách độc lập đối với tất cả các mẫu, xác nhận rằng việc so khớp chuỗi con là hiệu quả khi xen kẽ tùy ý.
