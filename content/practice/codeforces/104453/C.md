---
title: "CF 104453C - \u0411\u0438\u0442\u043e\u0432\u044b\u0435 \u043e\u043f\u0435\u0440\u0430\u0446\u0438\u0438"
description: "Chúng ta có hai chuỗi nhị phân, mỗi chuỗi có độ dài chính xác là N. Chúng có thể chứa các số 0 đứng đầu, do đó “dạng viết” của chúng không nhất thiết phải là biểu diễn nhị phân chuẩn mực của chúng."
date: "2026-06-30T14:32:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104453
codeforces_index: "C"
codeforces_contest_name: "ICPC Central Russia Regional Qualyfing Round, 2021"
rating: 0
weight: 104453
solve_time_s: 80
verified: true
draft: false
---

[CF 104453C - \u0411\u0438\u0442\u043e\u0432\u044b\u0435 \u043e\u043f\u0435\u0440\u0430\u0446\u0438\u0438](https://codeforces.com/problemset/problem/104453/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai chuỗi nhị phân, mỗi chuỗi có độ dài chính xác là N. Chúng có thể chứa các số 0 đứng đầu, do đó “dạng viết” của chúng không nhất thiết phải là biểu diễn nhị phân chuẩn mực của chúng. Nhiệm vụ của chúng tôi là tính toán XOR theo bit, từng vị trí và sau đó xuất chuỗi nhị phân kết quả sau khi loại bỏ bất kỳ số 0 đứng đầu nào. 

Về mặt khái niệm, chúng ta đang căn chỉnh hai mảng bit có độ dài bằng nhau và tạo ra mảng thứ ba trong đó mỗi vị trí là 1 nếu các bit đầu vào khác nhau và 0 nếu chúng giống nhau. Bước cuối cùng chỉ là định dạng: chúng ta không được in các số 0 ở đầu không cần thiết, có nghĩa là câu trả lời phải biểu thị cùng một giá trị nhị phân ở dạng ngắn nhất, ngoại trừ trường hợp đặc biệt khi kết quả bằng 0 phải xuất ra một`0`. 

Kích thước đầu vào N có thể lớn tới 100.000. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào liên tục chuyển đổi chuỗi thành số nguyên và thực hiện số học bằng cách sử dụng các phép toán số nguyên lớn đơn giản trong các cấu trúc cấp cao hơn, vì việc chuyển đổi lặp lại hoặc nối chuỗi từng bit bên trong các vòng lặp đắt tiền có nguy cơ xảy ra hành vi bậc hai. Quét tuyến tính trên các chuỗi là mục tiêu phức tạp an toàn duy nhất. 

Trường hợp cạnh tinh tế xuất hiện khi kết quả XOR toàn bằng 0. Ví dụ: nếu cả hai đầu vào giống hệt nhau, chẳng hạn như`0001`XOR`0001`, kết quả thô là`0000`, nhưng đầu ra phải là`0`, không phải là một chuỗi rỗng. Việc triển khai bất cẩn loại bỏ các số 0 đứng đầu mà không kiểm tra khoảng trống sẽ không in được gì không chính xác. 

Một trường hợp khác xuất phát từ các số 0 đứng đầu trong chính đầu vào. Ví dụ,`0001`Và`0001`vẫn nên sản xuất`0`, không`0000`hoặc một dòng trống. Điều này củng cố rằng việc chuẩn hóa chỉ phải xảy ra sau khi tính toán XOR chứ không phải trước đó. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ coi cả hai chuỗi nhị phân là số: chuyển đổi chúng thành số nguyên, áp dụng XOR và chuyển đổi trở lại thành nhị phân. Điều này đúng về mặt logic và cực kỳ nhỏ gọn trong mã. Tuy nhiên, bước chuyển đổi cho N lớn liên quan đến việc phân tích cú pháp tối đa 100.000 ký tự trên mỗi chuỗi và các chuyển đổi lặp lại hoặc các phép toán số nguyên lớn không hiệu quả có thể làm giảm hiệu suất tùy thuộc vào các ràng buộc về ngôn ngữ triển khai và thời gian chạy. Trong Python, nó vẫn tuyến tính, nhưng trong môi trường lập trình cạnh tranh nghiêm ngặt, giải pháp mong muốn sẽ tránh được chi phí phân tích cú pháp không cần thiết và phân bổ bổ sung. 

Một cách tiếp cận trực tiếp và đáng tin cậy hơn là xử lý từng chuỗi ký tự. Vì XOR trên các bit độc lập giữa các vị trí nên mỗi bit đầu ra chỉ phụ thuộc vào các bit đầu vào tương ứng. Chúng ta có thể quét từ trái sang phải, tính toán XOR một cách nhanh chóng và sau đó loại bỏ các số 0 đứng đầu khỏi kết quả. Quan sát quan trọng là chúng ta hoàn toàn không cần lưu trữ các biểu diễn số trung gian; các chuỗi đã mã hóa mọi thứ chúng ta cần. 

Điều này làm giảm vấn đề xuống còn một bước tuyến tính duy nhất, sau đó là một bước cắt xén đơn giản. Chúng tôi cũng tránh xây dựng các số nguyên tạm thời lớn hoặc nối chuỗi lặp lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (chuyển đổi int) | O(N) | O(N) | Đã chấp nhận | 
| Quét XOR chuỗi trực tiếp | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên N và hai xâu nhị phân A và B. 

Giá trị của N không bắt buộc phải có ngoài việc xác thực đầu vào vì cả hai chuỗi đều đã mang thông tin về độ dài rõ ràng. 
2. Khởi tạo một danh sách trống để lưu trữ các ký tự kết quả. 

Việc sử dụng danh sách thay vì nối chuỗi lặp lại là rất quan trọng vì chuỗi Python là bất biến và việc nối chuỗi lặp lại sẽ làm giảm hiệu suất xuống O(N²). 
3. Lặp lại các chỉ số từ 0 đến N − 1. 

Tại mỗi chỉ số, so sánh A[i] và B[i]. Nếu chúng khác nhau, hãy nối thêm`'1'`vào danh sách kết quả, nếu không thì nối thêm`'0'`. 
4. Chuyển danh sách thành chuỗi. 

Tại thời điểm này, chúng ta có kết quả XOR đầy đủ nhưng nó có thể chứa các số 0 đứng đầu. 
5. Loại bỏ các số 0 ở đầu chuỗi. 

Điều này được thực hiện bằng cách tìm sự xuất hiện đầu tiên của`'1'`. Nếu không có ký tự như vậy tồn tại thì kết quả hoàn toàn bằng 0. 
6. Nếu việc loại bỏ dẫn đến một chuỗi trống, hãy xuất`'0'`. 

Điều này xử lý trường hợp đặc biệt trong đó tất cả các bit bị hủy bỏ. 
7. Nếu không thì in chuỗi đã được cắt bớt. 

### Tại sao nó hoạt động 

XOR được xác định độc lập trên từng vị trí bit: bit đầu ra chỉ phụ thuộc vào hai bit đầu vào có cùng chỉ số. Điều này có nghĩa là bài toán không có sự phụ thuộc vị trí chéo nên chỉ cần thực hiện một lần là đủ. Chuyển đổi toàn cục duy nhất là định dạng: loại bỏ các số 0 đứng đầu, không làm thay đổi giá trị số được biểu thị bằng chuỗi bit. Thuật toán duy trì tính chính xác trên mỗi bit bằng cách xây dựng và chỉ điều chỉnh biểu diễn ở cuối. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n = int(input().strip())
    a = input().strip()
    b = input().strip()

    res = []

    for i in range(n):
        if a[i] == b[i]:
            res.append('0')
        else:
            res.append('1')

    s = ''.join(res)

    # remove leading zeros
    i = 0
    while i < len(s) and s[i] == '0':
        i += 1

    ans = s[i:]
    if ans == "":
        ans = "0"

    print(ans)

if __name__ == "__main__":
    main()
```Việc thực hiện giữ logic tuyến tính nghiêm ngặt. Vòng lặp xây dựng kết quả XOR mà không có bất kỳ chuyển đổi số trung gian nào. Việc sử dụng danh sách để tích lũy sẽ tránh việc phân bổ lại chuỗi lặp đi lặp lại. Sau bước quét chính, lần quét tuyến tính thứ hai sẽ loại bỏ các số 0 đứng đầu. Điều kiện cuối cùng đảm bảo tính chính xác khi kết quả XOR bằng 0 ở mọi nơi. 

Một sai lầm phổ biến là quên rằng việc loại bỏ các số 0 ở đầu có thể tạo ra một chuỗi trống. Một vấn đề tế nhị khác là cố gắng sử dụng`int(a, 2) ^ int(b, 2)`mà không tính đến việc các chuỗi cực lớn vẫn cần được phân tích cú pháp đầy đủ; trong khi Python hỗ trợ các số nguyên lớn, điều này gây ra chi phí không cần thiết so với xử lý bitwise trực tiếp. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
N = 2
A = 11
B = 10
```Xây dựng XOR từng bước: 

| tôi | A[i] | B[i] | Kết quả XOR | 
| --- | --- | --- | --- | 
| 0 | 1 | 1 | 0 | 
| 1 | 1 | 0 | 1 | 

Kết quả trước khi cắt tỉa:`01`Sau khi loại bỏ các số 0 đứng đầu:`1`Điều này xác nhận rằng việc loại bỏ số 0 ở đầu sẽ thay đổi cách trình bày chứ không phải giá trị. 

### Mẫu 2 

đầu vào:```
N = 4
A = 0011
B = 1100
```| tôi | A[i] | B[i] | Kết quả XOR | 
| --- | --- | --- | --- | 
| 0 | 0 | 1 | 1 | 
| 1 | 0 | 1 | 1 | 
| 2 | 1 | 0 | 1 | 
| 3 | 1 | 0 | 1 | 

Kết quả trước khi cắt tỉa:`1111`Sau khi cắt tỉa:`1111`Không có số 0 đứng đầu tồn tại nên đầu ra không thay đổi. 

Những dấu vết này cho thấy thuật toán hoàn toàn dựa trên vị trí và không phụ thuộc vào việc giải thích các chuỗi dưới dạng số cho đến định dạng cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi vị trí được xử lý chính xác một lần để xây dựng XOR và một lần để cắt tỉa | 
| Không gian | O(N) | Chúng tôi lưu trữ chuỗi đầu ra có độ dài N trong trường hợp xấu nhất | 

Giải pháp này phù hợp một cách thoải mái trong giới hạn N lên tới 100.000, vì cả hai đường chuyền đều là tuyến tính và chỉ liên quan đến việc so sánh và nối thêm ký tự đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input().strip())
    a = input().strip()
    b = input().strip()

    res = []
    for i in range(n):
        res.append('0' if a[i] == b[i] else '1')

    s = ''.join(res)
    i = 0
    while i < len(s) and s[i] == '0':
        i += 1

    ans = s[i:]
    if ans == "":
        ans = "0"
    return ans

# provided samples
assert run("2\n11\n10\n") == "1", "sample 1"
assert run("4\n0011\n1100\n") == "1111", "sample 2"

# custom cases
assert run("1\n0\n0\n") == "0", "minimum size all zero"
assert run("1\n1\n0\n") == "1", "minimum size single bit"
assert run("5\n00000\n00000\n") == "0", "all equal zeros"
assert run("5\n10101\n10101\n") == "0", "all equal non-zero"
assert run("6\n000111\n111000\n") == "111111", "full complement"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Các số 0 bằng nhau 1-bit | 0 | trường hợp ranh giới tối thiểu | 
| Khác nhau 1 bit | 1 | độ chính xác XOR vị trí đơn | 
| tất cả các số không bằng nhau | 0 | cắt tỉa hoàn toàn bằng không | 
| giống nhau khác không | 0 | loại bỏ số 0 dẫn đầu sau khi hủy hoàn toàn | 
| mẫu bổ sung | 111111 | truyền bá XOR đầy đủ | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi tất cả các bit bị hủy bỏ. Đối với đầu vào`A = 0000`,`B = 0000`, chuỗi XOR được tính toán sẽ trở thành`0000`. Bước cắt xén sẽ loại bỏ mọi ký tự, tạo ra một chuỗi trống. Thuật toán kiểm tra rõ ràng điều này và trả về`0`, đảm bảo biểu diễn nhị phân hợp lệ. 

Một trường hợp khác là khi kết quả có số 0 đứng đầu nhưng không bị hủy hoàn toàn, chẳng hạn như`A = 0010`,`B = 0001`. Kết quả XOR là`0011`. Vòng lặp cắt chỉ loại bỏ các số 0 đứng đầu và giữ lại hậu tố có ý nghĩa, tạo ra`11`. Điều này xác nhận rằng logic cắt xén chỉ ảnh hưởng đến biểu diễn chứ không ảnh hưởng đến giá trị. 

Cuối cùng, khi N = 1, thuật toán suy biến thành một phép so sánh đơn. Logic vẫn hoạt động mà không cần cách viết đặc biệt, chứng tỏ rằng cách tiếp cận này thống nhất trên tất cả các kích thước đầu vào.
