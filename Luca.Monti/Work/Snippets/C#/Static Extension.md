```c#
namespace System
{
    public static class Extensions

    {

        #region Integer extensions
        #region Bitcount
        public static byte BitCount(this byte data)
        {


            byte count = 0;
            while (data > 0)
            {    
                count += (byte)(data & 1);                    
                data >>= 1;
            }
            return count;
            
        }

        public static byte BitCount(this Int16 data)
        {
            return (byte)(BitCount((byte)(data >> 8)) + BitCount((byte)(data & 0xff)));
        }

        public static byte BitCount(this Int32 data)
        {
            return (byte)(BitCount((Int16)(data >> 16)) + BitCount((Int16)(data & 0xffff)));
        }

        public static byte BitCount(this Int64 data)
        {
            return (byte)(BitCount((Int32)(data >> 32)) + BitCount((Int32)(data & 0xffffffff)));
        }
        #endregion


        public static bool IsNullOrEmpty(this string value)
        {
            return value is null || value.Equals("");
        }



        #endregion
    }
}```
