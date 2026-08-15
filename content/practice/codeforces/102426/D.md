---
title: "CF 102426D - \u5143\u7d20\u5468\u671f\u8868"
description: "Chúng ta cần đánh giá một số công thức hóa học và tính khối lượng phân tử tương đối của chúng. Công thức là một chuỗi các ký hiệu phần tử, trong đó ký hiệu phần tử bao gồm một chữ cái viết hoa và có thể một chữ cái viết thường."
date: "2026-08-12T19:22:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102426
codeforces_index: "D"
codeforces_contest_name: "The 7-th BIT Campus Programming Contest for Junior Grade Group"
rating: 0
weight: 102426
solve_time_s: 110
verified: true
draft: false
---

[CF 102426D - \u5143\u7d20\u5468\u671f\u8868](https://codeforces.com/problemset/problem/102426/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 50s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta cần đánh giá một số công thức hóa học và tính khối lượng phân tử tương đối của chúng. Công thức là một chuỗi các ký hiệu phần tử, trong đó ký hiệu phần tử bao gồm một chữ cái viết hoa và có thể một chữ cái viết thường. Con số ngay sau ký hiệu là số nguyên tử của nguyên tố đó. Nếu số bị bỏ qua, phần tử sẽ xuất hiện đúng một lần. 

Ví dụ,`H2O`có nghĩa là hai nguyên tử hydro và một nguyên tử oxy, vì vậy khối lượng phân tử của nó là`2 × 1.008 + 1 × 16 = 18.016`. Tương tự,`CO2`là`1 × 12.01 + 2 × 16 = 44.01`. 

Dữ liệu đầu vào chứa tối đa 200 công thức và mỗi công thức có độ dài tối đa là 1000. Số phần tử tối đa là 1000. Các giới hạn này đủ nhỏ để chúng ta chỉ cần xử lý mỗi ký tự một lần. Tổng lượng đầu vào tối đa là 200.000 ký tự, do đó giải pháp O(tổng chiều dài) chỉ thực hiện vài trăm nghìn thao tác phân tích cú pháp. Không có lý do gì để sử dụng lập trình động, thủ thuật băm hoặc bất kỳ kỹ thuật phức tạp nào hơn. 

Khó khăn không phải là số học. Trình phân tích cú pháp phải phân biệt giữa các chữ cái viết hoa, chữ thường thuộc cùng một ký hiệu và các chữ số thuộc về số lượng nguyên tử. Một công thức như`NaCl`chứa hai phần tử mặc dù không có số giữa chúng, trong khi`C12H22O11`chứa ba phần tử có số lượng nhiều chữ số. 

Một số trường hợp ranh giới có thể âm thầm phá vỡ quá trình triển khai. Vì`H`, câu trả lời là`1.008`, bởi vì số lượng bị bỏ qua có nghĩa là một nguyên tử. Trình phân tích cú pháp chỉ thêm một phần tử sau khi nhìn thấy một số sẽ bỏ qua phần tử đó một cách không chính xác. 

Vì`He`, câu trả lời là`4.003`. các`e`là một phần của biểu tượng phần tử, không phải là mã thông báo riêng biệt. Việc triển khai giả định mọi phần tử là một ký tự sẽ tra cứu`H`rồi xử lý sai`e`. 

Vì`O1000`, câu trả lời là`16000.000`. Số đếm có thể chứa ba chữ số, vì vậy chỉ đọc một chữ số sẽ biến 1000 thành 1. 

cho`NaCl`, câu trả lời là`58.440`. Không có số đếm rõ ràng nên cả hai phần tử đều có số đếm ngầm định là một. Trình phân tích cú pháp phải hoàn thành phần tử hiện tại trước khi chuyển sang chữ in hoa tiếp theo. 

Khối lượng nguyên tử là hằng số cố định. Bảng thông thường được sử dụng cho bài toán này đưa ra các giá trị như H = 1,008, He = 4,003, C = 12,01, O = 16 và tiếp tục đi qua tất cả 118 phần tử. Một giải pháp được công bố cho vấn đề chính xác này sử dụng cùng một bảng 118 mục. 

## Phương pháp tiếp cận 

Trình phân tích cú pháp brute-force theo nghĩa đen có thể liên tục kiểm tra phần còn lại của công thức bất cứ khi nào nó cần xác định vị trí kết thúc của mã thông báo phần tử hoặc số của nó. Đối với một công thức có độ dài L, việc quét lại như vậy có thể kiểm tra một hậu tố có độ dài khoảng L, sau đó là L - 1, v.v., đưa ra khoảng`1 + 2 + ... + L = L(L + 1) / 2`kiểm tra nhân vật. Với L = 1000, tức là có khoảng 500.500 lần kiểm tra cho một công thức. Ở mức tối đa T = 200, con số này đạt khoảng 100 triệu lượt kiểm tra ký tự, mức này tốn kém một cách không cần thiết trong giới hạn 1 giây. 

Cách tiếp cận bạo lực có hiệu quả vì mọi thông tin đều mang tính địa phương. Vấn đề là nó liên tục khám phá lại thông tin đã được sử dụng. 

Điều quan trọng cần lưu ý là công thức này có ngữ pháp rất nghiêm ngặt. Bất cứ khi nào chúng ta gặp một chữ in hoa, một phần tử mới sẽ bắt đầu. Có thể có nhiều nhất một chữ cái viết thường ngay sau nó. Sau ký hiệu có thể có 0 hoặc nhiều chữ số. Khi các chữ số đó kết thúc, chữ in hoa tiếp theo sẽ bắt đầu phần tử tiếp theo. Chúng ta không bao giờ cần phải nhìn lại hoặc quét lại bất cứ thứ gì. 

Điều đó cho phép chúng ta phân tích công thức bằng một con trỏ từ trái sang phải. Tại mỗi phần tử, chúng ta đọc ký hiệu của nó, đọc tất cả các chữ số theo sau để đếm, cộng`atomic_mass × count`đến câu trả lời và tiếp tục chính xác nơi phần tử tiếp theo bắt đầu. Mỗi ký tự được sử dụng một lần, do đó độ phức tạp giảm từ bậc hai xuống tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(L²) mỗi công thức | O(1) | Quá chậm trong trường hợp xấu nhất | 
| Tối ưu | O(L) mỗi công thức | O(1) ngoài bảng khối lượng cố định | Đã chấp nhận | 

Ở đây L là chiều dài của một công thức phân tử. Từ điển bảng tuần hoàn cố định chỉ có 118 mục từ, do đó kích thước của nó thực tế là không đổi. 

## Hướng dẫn thuật toán 

1. Lưu trữ khối lượng nguyên tử của mỗi nguyên tố hóa học vào một từ điển có ký hiệu của nó. Công thức cho các ký hiệu như`H`,`O`,`Na`, Và`Cl`, vì vậy việc tra cứu từ điển trực tiếp cho phép chúng ta lấy được từng khối ngay lập tức. 
2. Đặt con trỏ`i`đến đầu công thức và khởi tạo khối lượng phân tử`total`về không. Con trỏ luôn đánh dấu ký tự đầu tiên của phần tử tiếp theo chưa được xử lý. 
3. Đọc ký tự viết hoa tại`i`là phần đầu tiên của biểu tượng phần tử. Nếu ký tự tiếp theo tồn tại và là chữ thường, hãy thêm nó vào ký hiệu và tiến lên`i`. Điều này xử lý cả các ký hiệu một chữ cái như`C`và các ký hiệu gồm hai chữ cái như`Fe`. 
4. Bắt đầu từ vị trí hiện tại, sử dụng từng chữ số liên tiếp và xây dựng số lượng nguyên tử. Nếu không có chữ số nào xuất hiện, hãy đếm một. Sự vắng mặt của một số có ý nghĩa chính xác trong ký hiệu hóa học, vì vậy việc coi nó như một số sẽ tránh được trường hợp đặc biệt sau này. 
5. Tra cứu khối lượng nguyên tử của ký hiệu được phân tích cú pháp và thêm`mass × count`ĐẾN`total`. Tại thời điểm này, mọi ký tự thuộc phần tử này đã được sử dụng. 
6. Lặp lại cho đến khi`i`đến cuối công thức. Vì mỗi lần lặp cần ít nhất một ký tự nên con trỏ luôn di chuyển về phía trước và vòng lặp kết thúc. 
7. In`total`có ba chữ số sau dấu thập phân. Ba chữ số thập phân cho sai số thấp hơn nhiều so với yêu cầu`10^-3`sức chịu đựng. 

Tại sao nó hoạt động 

Điều bất biến là ngay trước mỗi lần lặp, mọi ký tự trước`i`đã được gán cho chính xác một lần xuất hiện phần tử và phần đóng góp của nó đã được thêm vào`total`. Vị trí hiện tại`i`do đó chính xác là sự bắt đầu của phần tử chưa được xử lý tiếp theo. Việc đọc một chữ cái viết hoa, chữ cái viết thường tùy chọn và các chữ số tiếp theo của nó sẽ thể hiện chính xác sự xuất hiện đầy đủ của phần tử đó. Sự đóng góp của nó sau đó được thêm vào đúng một lần. Bất biến vẫn đúng sau phép lặp và khi`i`đến cuối, mọi lần xuất hiện phần tử đều được xử lý chính xác một lần, vì vậy`total`là khối lượng phân tử. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

symbols = [
    "H", "He", "Li", "Be", "B", "C", "N", "O", "F", "Ne",
    "Na", "Mg", "Al", "Si", "P", "S", "Cl", "Ar", "K", "Ca",
    "Sc", "Ti", "V", "Cr", "Mn", "Fe", "Co", "Ni", "Cu", "Zn",
    "Ga", "Ge", "As", "Se", "Br", "Kr", "Rb", "Sr", "Y", "Zr",
    "Nb", "Mo", "Tc", "Ru", "Rh", "Pd", "Ag", "Cd", "In", "Sn",
    "Sb", "Te", "I", "Xe", "Cs", "Ba", "La", "Ce", "Pr", "Nd",
    "Pm", "Sm", "Eu", "Gd", "Tb", "Dy", "Ho", "Er", "Tm", "Yb",
    "Lu", "Hf", "Ta", "W", "Re", "Os", "Ir", "Pt", "Au", "Hg",
    "Tl", "Pb", "Bi", "Po", "At", "Rn", "Fr", "Ra", "Ac", "Th",
    "Pa", "U", "Np", "Pu", "Am", "Cm", "Bk", "Cf", "Es", "Fm",
    "Md", "No", "Lr", "Rf", "Db", "Sg", "Bh", "Hs", "Mt", "Ds",
    "Rg", "Cn", "Nh", "Fl", "Mc", "Lv", "Ts", "Og"
]

masses = [
    1.008, 4.003, 6.941, 9.012, 10.81, 12.01, 14.01, 16.0, 19.0, 20.18,
    22.99, 24.31, 26.98, 28.09, 30.97, 32.07, 35.45, 39.95, 39.10, 40.08,
    44.96, 47.88, 50.94, 52.0, 54.94, 55.85, 58.93, 58.69, 63.55, 65.39,
    69.72, 72.59, 74.92, 78.96, 79.90, 83.80, 85.47, 87.62, 88.91, 91.22,
    92.91, 95.94, 97.91, 101.1, 102.9, 106.4, 107.9, 112.4, 114.8, 118.7,
    121.8, 127.6, 126.9, 131.3, 132.9, 137.3, 138.9, 140.1, 140.9, 144.2,
    144.9, 150.4, 152.0, 157.3, 158.9, 162.5, 164.9, 167.3, 168.9, 173.0,
    175.0, 178.5, 180.9, 183.9, 186.2, 190.2, 192.2, 195.1, 197.0, 200.6,
    204.4, 207.2, 209.0, 209.0, 210.0, 222.0, 223.0, 226.0, 227.0, 232.0,
    231.0, 238.0, 237.1, 244.1, 243.1, 247.1, 247.1, 252.1, 252.1, 257.1,
    258.1, 259.1, 262.1, 265.1, 268.1, 271.1, 270.1, 277.2, 276.2, 281.2,
    280.2, 285.2, 284.2, 289.2, 288.2, 293.2, 294.2, 294.2
]

MASS = dict(zip(symbols, masses))

def molecular_mass(formula):
    n = len(formula)
    i = 0
    total = 0.0

    while i < n:
        # Every element starts with an uppercase letter.
        symbol = formula[i]
        i += 1

        # A second lowercase letter belongs to the same symbol.
        if i < n and formula[i].islower():
            symbol += formula[i]
            i += 1

        # Read the complete decimal count.
        count = 0
        while i < n and formula[i].isdigit():
            count = count * 10 + (ord(formula[i]) - ord('0'))
            i += 1

        # No explicit count means exactly one atom.
        if count == 0:
            count = 1

        total += MASS[symbol] * count

    return total

def solve():
    t = int(input())
    for _ in range(t):
        formula = input().strip()
        print(f"{molecular_mass(formula):.3f}")

if __name__ == "__main__":
    solve()
```các`symbols`Và`masses`mảng mô tả cùng một bảng tuần hoàn theo thứ tự số nguyên tử.`dict(zip(...))`biến chúng thành tra cứu trực tiếp từ ký hiệu đến khối lượng, do đó việc phân tích cú pháp không yêu cầu tìm kiếm qua tất cả 118 phần tử. Các giá trị trên tuân theo bảng cố định được sử dụng trong các giải pháp được chấp nhận cho vấn đề này. 

Bên trong`molecular_mass`,`i`là con trỏ phân tích cú pháp duy nhất. Ký tự đầu tiên của mọi phần tử đều là chữ hoa nên nó có thể bắt đầu ngay lập tức một biểu tượng. Việc kiểm tra chữ thường sử dụng`i < n`trước khi lập chỉ mục, điều này ngăn cản việc truy cập ngoài phạm vi khi việc kiểm tra hai chữ cái xảy ra ở cuối công thức. 

Vòng lặp chữ số được cố tình tách biệt khỏi phân tích ký hiệu. Một công thức như`C123`đầu tiên tạo ra biểu tượng`C`, sau đó đọc`1`,`2`, Và`3`như một số 123. Biểu thức`count = count * 10 + digit`là cách tiêu chuẩn để xây dựng một số nguyên thập phân từ các ký tự của nó. 

sử dụng`count == 0`vì chỉ báo cho một số bị bỏ qua là an toàn vì câu lệnh đảm bảo các công thức hợp lệ và mọi số phần tử rõ ràng đều dương. Một số lá bị bỏ qua`count`ở mức 0, sau đó nó được đổi thành một. 

Số nguyên Python không bị tràn và khối lượng phân tử lớn nhất có thể nằm trong phạm vi giá trị dấu phẩy động. In bằng`.3f`mang lại độ chính xác cần thiết đồng thời tạo ra định dạng đầu ra nhất quán. 

## Ví dụ đã hoạt động 

### Mẫu 1: H2O 

Công thức được xử lý từ trái qua phải. 

|`i`trước | Biểu tượng | Đếm | Đóng góp |`total`| 
| --- | --- | --- | --- | --- | 
| 0 | H | 2 | 2 × 1,008 = 2,016 | 2.016 | 
| 2 | Ồ | 1 | 1 × 16 = 16.000 | 18.016 | 

Sau khi đọc`H`, trình phân tích cú pháp sẽ nhìn thấy chữ số`2`, do đó số đếm là 2. Chữ in hoa tiếp theo bắt đầu`O`, không có số theo sau nên số đếm của nó trở thành 1. Câu trả lời cuối cùng là`18.016`. 

### Mẫu 2: CO2 

|`i`trước | Biểu tượng | Đếm | Đóng góp |`total`| 
| --- | --- | --- | --- | --- | 
| 0 | C | 1 | 1 × 12,01 = 12,010 | 12.010 | 
| 1 | Ồ | 2 | 2 × 16 = 32.000 | 44.010 | 

Phần tử đầu tiên không có số rõ ràng nên trình phân tích cú pháp gán cho nó số đếm một. các`O`được theo sau bởi`2`, trở thành số đếm đầy đủ của nó. Kết quả in ra là`44.010`, tương đương với mẫu`44.01`và thỏa mãn khả năng chịu lỗi yêu cầu. 

### Dấu vết bổ sung: C12H22O11 

|`i`trước | Biểu tượng | Đếm | Đóng góp |`total`| 
| --- | --- | --- | --- | --- | 
| 0 | C | 12 | 12 × 12,01 = 144,120 | 144.120 | 
| 3 | H | 22 | 22 × 1,008 = 22,176 | 166.296 | 
| 6 | Ồ | 11 | 11 × 16 = 176.000 | 342.296 | 

Dấu vết này giải thích tại sao vòng lặp chữ số phải tiếp tục cho đến khi toàn bộ số được sử dụng hết. Chỉ đọc chữ số đầu tiên sẽ tạo ra số đếm hoàn toàn khác nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(L) trên mỗi công thức, O(tổng chiều dài đầu vào) tổng thể | Mỗi ký tự được kiểm tra một lần và mỗi lần tra cứu phần tử là O(1) | 
| Không gian | O(1) không gian phụ trợ | Chỉ sử dụng một số lượng biến phân tích không đổi; bảng khối 118 mục đã được cố định | 

Trong tất cả các trường hợp thử nghiệm, tổng độ dài công thức tối đa là 200.000 ký tự. Do đó, quét tuyến tính chỉ thực hiện công việc phân tích cú pháp O(200.000), dễ dàng thực hiện trong giới hạn thời gian đã nêu. Việc sử dụng bộ nhớ cũng rất nhỏ vì bản thân công thức và bảng 118 phần tử cố định là bộ nhớ duy nhất có liên quan. 

## Trường hợp thử nghiệm```python
import sys
import io

symbols = [
    "H", "He", "Li", "Be", "B", "C", "N", "O", "F", "Ne",
    "Na", "Mg", "Al", "Si", "P", "S", "Cl", "Ar", "K", "Ca",
    "Sc", "Ti", "V", "Cr", "Mn", "Fe", "Co", "Ni", "Cu", "Zn",
    "Ga", "Ge", "As", "Se", "Br", "Kr", "Rb", "Sr", "Y", "Zr",
    "Nb", "Mo", "Tc", "Ru", "Rh", "Pd", "Ag", "Cd", "In", "Sn",
    "Sb", "Te", "I", "Xe", "Cs", "Ba", "La", "Ce", "Pr", "Nd",
    "Pm", "Sm", "Eu", "Gd", "Tb", "Dy", "Ho", "Er", "Tm", "Yb",
    "Lu", "Hf", "Ta", "W", "Re", "Os", "Ir", "Pt", "Au", "Hg",
    "Tl", "Pb", "Bi", "Po", "At", "Rn", "Fr", "Ra", "Ac", "Th",
    "Pa", "U", "Np", "Pu", "Am", "Cm", "Bk", "Cf", "Es", "Fm",
    "Md", "No", "Lr", "Rf", "Db", "Sg", "Bh", "Hs", "Mt", "Ds",
    "Rg", "Cn", "Nh", "Fl", "Mc", "Lv", "Ts", "Og"
]

masses = [
    1.008, 4.003, 6.941, 9.012, 10.81, 12.01, 14.01, 16.0, 19.0, 20.18,
    22.99, 24.31, 26.98, 28.09, 30.97, 32.07, 35.45, 39.95, 39.10, 40.08,
    44.96, 47.88, 50.94, 52.0, 54.94, 55.85, 58.93, 58.69, 63.55, 65.39,
    69.72, 72.59, 74.92, 78.96, 79.90, 83.80, 85.47, 87.62, 88.91, 91.22,
    92.91, 95.94, 97.91, 101.1, 102.9, 106.4, 107.9, 112.4, 114.8, 118.7,
    121.8, 127.6, 126.9, 131.3, 132.9, 137.3, 138.9, 140.1, 140.9, 144.2,
    144.9, 150.4, 152.0, 157.3, 158.9, 162.5, 164.9, 167.3, 168.9, 173.0,
    175.0, 178.5, 180.9, 183.9, 186.2, 190.2, 192.2, 195.1, 197.0, 200.6,
    204.4, 207.2, 209.0, 209.0, 210.0, 222.0, 223.0, 226.0, 227.0, 232.0,
    231.0, 238.0, 237.1, 244.1, 243.1, 247.1, 247.1, 252.1, 252.1, 257.1,
    258.1, 259.1, 262.1, 265.1, 268.1, 271.1, 270.1, 277.2, 276.2, 281.2,
    280.2, 285.2, 284.2, 289.2, 288.2, 293.2, 294.2, 294.2
]

MASS = dict(zip(symbols, masses))

def molecular_mass(formula):
    i = 0
    total = 0.0
    n = len(formula)

    while i < n:
        symbol = formula[i]
        i += 1

        if i < n and formula[i].islower():
            symbol += formula[i]
            i += 1

        count = 0
        while i < n and formula[i].isdigit():
            count = count * 10 + int(formula[i])
            i += 1

        if count == 0:
            count = 1

        total += MASS[symbol] * count

    return total

def run(inp: str) -> str:
    data = io.StringIO(inp)
    t = int(data.readline())
    output = []

    for _ in range(t):
        formula = data.readline().strip()
        output.append(f"{molecular_mass(formula):.3f}")

    return "\n".join(output) + "\n"

# Provided samples
assert run("""2
H2O
CO2
""") == """18.016
44.010
""", "provided samples"

# Minimum-size input and implicit count
assert run("""1
H
""") == """1.008
""", "single-letter element with omitted count"

# Two-letter element at the end, with omitted count
assert run("""1
He
""") == """4.003
""", "two-letter element at formula boundary"

# Maximum allowed count
assert run("""1
O1000
""") == """16000.000
""", "three-digit count equal to 1000"

# Multiple elements and multiple digits
assert run("""1
C6H12O6
""") == """180.156
""", "multi-element formula with multi-digit counts"

# Maximum formula length: 200 copies of H1000 gives exactly 1000 characters
max_formula = "H1000" * 200
assert len(max_formula) == 1000
assert run("1\n" + max_formula + "\n") == "201600.000\n", \
    "maximum formula length"

# Consecutive two-letter and one-letter symbols without counts
assert run("""1
NaCl
""") == """58.440
""", "adjacent elements without explicit counts"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`H`|`1.008`| Công thức kích thước tối thiểu và số ẩn một | 
|`He`|`4.003`| Ký hiệu hai chữ cái và ranh giới cuối chuỗi | 
|`O1000`|`16000.000`| Số phần tử tối đa được phép và phân tích cú pháp nhiều chữ số | 
|`C6H12O6`|`180.156`| Một số phần tử và số lượng nhiều chữ số | 
|`H1000`lặp lại 200 lần |`201600.000`| Độ dài công thức tối đa và các phần tử giống hệt nhau lặp đi lặp lại | 
|`NaCl`|`58.440`| Các phần tử liên tiếp không có số lượng rõ ràng | 

## Vỏ cạnh 

Đối với một số lượng bị bỏ qua như`H`, trình phân tích cú pháp sẽ đọc`H`, thấy rằng ký tự tiếp theo không tồn tại và rời đi`count`ở mức không. Sau đó nó chuyển số 0 đó thành một và thêm`1 × 1.008`, sản xuất`1.008`. Điều này ngăn ngừa lỗi phổ biến khi yêu cầu mọi phần tử phải có chữ số theo sau. 

Đối với một ký hiệu gồm hai chữ cái như`He`, trình phân tích cú pháp đầu tiên sẽ tiêu thụ`H`, kiểm tra ký tự tiếp theo, thấy chữ thường`e`, và mở rộng ký hiệu thành`He`. Con trỏ sau đó sẽ đến cuối nên số đếm là một. Tra cứu trả về 4.003 và câu trả lời là`4.003`. Việc kiểm tra ranh giới rõ ràng trước khi đọc ký tự chữ thường là điều làm cho phần cuối của chuỗi được an toàn. 

Vì`O1000`, trình phân tích cú pháp sẽ đọc`O`và sau đó thực hiện vòng lặp chữ số bốn lần. Số lượng trung gian là 1, 10, 100 và cuối cùng là 1000. Phần đóng góp là`1000 × 16 = 16000`, vì vậy đầu ra là`16000.000`. Trình phân tích cú pháp chỉ đọc một chữ số sẽ thất bại trong trường hợp này ngay lập tức. 

Vì`NaCl`, trình phân tích cú pháp đầu tiên sẽ đọc`Na`và gán số một vì không có chữ số nào theo sau. Con trỏ lúc này được đặt ở vị trí`C`, bắt đầu một phần tử mới. Sau đó nó đọc`Cl`, một lần nữa với số đếm ngầm định là một. Kết quả là`22.99 + 35.45 = 58.44`, được in dưới dạng`58.440`. Điều này thực hiện quá trình chuyển đổi trực tiếp từ phần tử này sang phần tử tiếp theo mà không có bất kỳ dấu phân cách số nào. 

Vì`C6H12O11`, bộ phân tích cú pháp phải phân biệt dãy số`12`từ hai đại lượng riêng biệt. Sau khi đọc`H`, nó tiêu thụ cả hai chữ số trước khi tiếp tục`O`. Phần đóng góp của hydro là`12 × 1.008 = 12.096`. Cơ chế xử lý tương tự`O11`. Bất biến dựa trên con trỏ đảm bảo rằng không có chữ số nào vô tình được hiểu là một phần của phần tử theo sau. 

Đối với công thức có độ dài tối đa bao gồm 200 bản sao`H1000`, mỗi khối năm ký tự đóng góp`1000 × 1.008 = 1008`. Có 200 khối như vậy nên khối lượng cuối cùng là`200 × 1008 = 201600`. Mỗi lần lặp lại sử dụng chính xác một lần xuất hiện phần tử hoàn chỉnh, chứng tỏ rằng trình phân tích cú pháp tuyến tính vẫn chính xác ngay cả khi đầu vào đạt đến độ dài tối đa.
