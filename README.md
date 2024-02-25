# Encendido-y-apagado-led-intermitente
#include <stdint.h>

// PERIPHERAL & BUS BASE ADDRESS

#define PERIPHERAL_BASE_ADDRESS          0x40000000U
#define AHB_BASE_ADDRESS                 (PERIPHERAL_BASE_ADDRESS + 0x00020000U)

// RCC BASE ADDRESS
#define RCC_BASE_ADDRESS                 (AHB_BASE_ADDRESS + 0x00001000U)
#define RCC_IOPENR_ADDRESS               (RCC_BASE_ADDRESS + 0x0000002CU)

//IOPORT BASE ADDRESS
#define IOPORT_ADDRESS                   (PERIPHERAL_BASE_ADDRESS + 0x10000000U)

// GPIO BASE  & SPECIFIC ADDRESS

#define GPIOA_BASE_ADDRESS               (IOPORT_ADDRESS     + 0x00000000U)
#define GPIOA_MODER_REG                  (GPIOA_BASE_ADDRESS + 0x00000000U)
#define GPIOA_ODR_REG                    (GPIOA_BASE_ADDRESS + 0x00000014U)

#define GPIOC_BASE_ADDRESS               (IOPORT_ADDRESS     + 0x000000800U)
#define GPIOC_MODER_REG                  (GPIOC_BASE_ADDRESS + 0x00000000U)
#define GPIOC_ODR_REG                    (GPIOC_BASE_ADDRESS + 0x00000014U)

#define GPIOC_IDR_REG                     (GPIOC_BASE_ADDRESS + 0x00000010U)

#define GPIOC_PUPDR_REG                    (GPIOC_BASE_ADDRESS + 0x0000000CU)
void delay_ms(uint16_t n);


int main(void)
{
	uint32_t *ptr_rcc_iopenr   = RCC_IOPENR_ADDRESS;
	uint32_t *ptr_gpioa_moder  = GPIOA_MODER_REG;
	uint32_t *ptr_gpioa_odr    = GPIOA_ODR_REG;

//	uint32_t *ptr_rcc_iopenr   =  RCC_IOPENR_ADDRESS;
	uint32_t *ptr_gpioc_moder  =  GPIOC_MODER_REG;
	uint32_t *ptr_gpioc_odr    =  GPIOC_ODR_REG;
	uint32_t *ptr_gpioc_idr    =  GPIOC_IDR_REG;
	uint32_t *ptr_gpioc_pupdr   =  GPIOC_PUPDR_REG;

	 uint8_t pulse_counter = 0; // Variable para contar pulsos en PA11
	/* dereferencing */

	//enable reloj

	// Enable reloj GPIOA
	   *ptr_rcc_iopenr   |= 1<<0;

	// Enable reloj GPIOC
		*ptr_rcc_iopenr  |= 1<<2;

	//configurador MODER
		/* For PA8 */
	    *ptr_gpioa_moder &= ~(1<<17);
	    *ptr_gpioa_moder &= ~(1<<25);
	//    *ptr_gpioa_odr   |=   1<<5;

	    /* PC12 Boton */
	    *ptr_gpioc_moder &= ~(1<<24);
	    *ptr_gpioc_moder &= ~(1<<25);
        *ptr_gpioc_idr   |=   1<<12;

        // PC11, Boton COUNT

               *ptr_gpioc_moder &= ~(1<<22);
        	   *ptr_gpioc_moder &= ~(1<<23);
               *ptr_gpioc_idr   |=   1<<11;


        //GPIOC Pull Up
        //PC12
        *ptr_gpioc_pupdr |= 1<<24;


        //GPIOC Pull Up
        //PC11
        *ptr_gpioc_pupdr |= 1<<22;

while (1)
{
 	 if (//*ptr_gpioc_pupdr & (1<<24))
 		 *ptr_gpioc_idr & (1<<12))
 	                               {

 	 *ptr_gpioa_odr   &=   ~(1<<8);
 	  delay_ms(500);
 	 *ptr_gpioa_odr   |=     1<<8;
      delay_ms(500);

 	                               }
 	else   // comando else donde el pulsador no esta conectado haga la funcion de encendido intermitente cada 200ms
		{

 	    *ptr_gpioa_odr   |=    1<<8;
 		 delay_ms(200);
 		*ptr_gpioa_odr   &=  ~(1<<8);
 		 delay_ms(200);

		}
 	 if (*ptr_gpioc_idr & (1<<11) ){
 		// *ptr_gpioa_odr   |=1<<12;
 		*ptr_gpioa_odr   &=  ~(1<<12);
 	   pulse_counter++;   // comando que suma 1 al valor  uint8_t pulse_counter
 	 }
 	 else {
 		*ptr_gpioa_odr   |=1<<12;

 	 }
 	  delay_ms(100);
   }
}

void delay_ms(uint16_t n)
	{
		uint16_t i;
		for(; n>0; n--)
			for(i=0; i<140; i++);  // estructura basica que se acerca a 1 segundo real
	}

